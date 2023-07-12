一个还算满意的缩放ImageView。
暂时先把代码贴上，以后慢慢写讲解。
```
class ZoomImageView(context: Context?, attrs: AttributeSet?) : ImageView(context, attrs)
        , ScaleGestureDetector.OnScaleGestureListener
        , View.OnTouchListener
        , ViewTreeObserver.OnGlobalLayoutListener {
    private val tag = "ZoomImageView"

    private var mScaleGestureDetector: ScaleGestureDetector
    private var mGestureDetector: GestureDetector

    private val scaleMax = 100.0f
    private val scaleMin = 0.1f
    private val scaleDoubleMid = 4.0f
    private val scaleDoubleMax = 8.0f
    private var initScale = 1.0f

    private val matrixValues = FloatArray(9)
    private var once = true
    private var mScaleMatrix = Matrix()

    private var mLastX = 0f
    private var mLastY = 0f

    private var isCanDrag = true
    private var lastPointerCount = 0
    private var isCheckTopAndBottom = false
    private var isCheckLeftAndRight = false
    private var mTouchSlop = 0

    private var isAutoScale = false

    private var isShrink = false

    init {
        super.setScaleType(ScaleType.MATRIX)
        setBackgroundColor(Color.parseColor("#00ffffff"))

        //缩放手势
        mScaleGestureDetector = ScaleGestureDetector(context, this)

        //手势
        mGestureDetector = GestureDetector(context,
                object : SimpleOnGestureListener() {
                    override fun onDoubleTap(e: MotionEvent): Boolean {
                        if (isAutoScale)
                            return true

                        val x = e.x
                        val y = e.y
                        Log.e("DoubleTap", getScale().toString() + " , " + initScale)
                        if (getScale() < scaleDoubleMid) {
                            this@ZoomImageView.postDelayed(
                                    AutoScaleRunnable(scaleDoubleMid, x, y), 16
                            )
                            isAutoScale = true
                        } else if (getScale() >= scaleDoubleMid && getScale() < scaleDoubleMax) {
                            this@ZoomImageView.postDelayed(
                                    AutoScaleRunnable(scaleDoubleMax, x, y), 16
                            )
                            isAutoScale = true
                        } else {
                            this@ZoomImageView.postDelayed(
                                    AutoScaleRunnable(initScale, x, y), 16
                            )
                            isAutoScale = true
                        }
                        return true
                    }

                    override fun onSingleTapConfirmed(e: MotionEvent?): Boolean {
                        this@ZoomImageView.postDelayed(HideZoomImageView(), 16)
                        return super.onSingleTapConfirmed(e)
                    }
                })
        mTouchSlop = ViewConfiguration.get(context).scaledTouchSlop

        this.setOnTouchListener(this)
    }

    override fun onScale(detector: ScaleGestureDetector?): Boolean {
        val scale = getScale()
        var scaleFactor = detector?.scaleFactor
        if (drawable == null || scaleFactor == null) return true

        //缩放范围控制
        if (enlarge(scale, scaleFactor)
                || enSmall(scale, scaleFactor)
        ) {
            //最大最小值判断
            if (scaleFactor * scale < scaleMin) {
                scaleFactor = scaleMin / scale
            }
            if (scaleFactor * scale > scaleMax) {
                scaleFactor = scaleMax / scale
            }

            //设置缩放比例
            mScaleMatrix.postScale(scaleFactor, scaleFactor, width / 2F, height / 2F)

            mScaleMatrix.postScale(
                    scaleFactor,
                    scaleFactor,
                    detector!!.focusX * 1.0f,
                    detector.focusY * 1.0f
            )

            putPicCenterOnScale()

            imageMatrix = mScaleMatrix
            Log.d(tag, "scaleFactor$scaleFactor")
            Log.d(tag, "scale$scale")
        }
        return true
    }

    override fun onScaleBegin(detector: ScaleGestureDetector?): Boolean {
        return true
    }

    override fun onScaleEnd(detector: ScaleGestureDetector?) {

    }

    override fun onTouch(v: View?, event: MotionEvent?): Boolean {
        if (mGestureDetector.onTouchEvent(event))
            return true

        mScaleGestureDetector.onTouchEvent(event)

        var x = 0f
        var y = 0f
        val pointerCount = event!!.pointerCount

        for (i in 0 until pointerCount) {
            x += event.getX(i)
            y += event.getY(i)
        }
        x /= pointerCount
        y /= pointerCount

        if (pointerCount != lastPointerCount) {
            isCanDrag = false
            mLastX = x
            mLastY = y
        }


        lastPointerCount = pointerCount

        when (event.action) {
            MotionEvent.ACTION_MOVE -> {
                var dx = x - mLastX
                var dy = y - mLastY

                if (!isCanDrag) {
                    isCanDrag = isCanDrag(dx, dy)
                }
                if (isCanDrag) {
                    val rectF = getMatrixRectF()
                    if (drawable != null) {
                        isCheckTopAndBottom = true
                        isCheckLeftAndRight = isCheckTopAndBottom
                        // 如果宽度小于屏幕宽度，则禁止左右移动
                        if (rectF.width() < width) {
                            dx = 0f
                            isCheckLeftAndRight = false
                        }
                        // 如果高度小雨屏幕高度，则禁止上下移动
                        if (rectF.height() < height) {
                            dy = 0f
                            isCheckTopAndBottom = false
                        }
                        mScaleMatrix.postTranslate(dx, dy)
                        checkMatrixBounds()
                        imageMatrix = mScaleMatrix
                    }
                }
                mLastX = x
                mLastY = y
            }

            MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                lastPointerCount = 0
            }
        }

        return true
    }

    override fun onGlobalLayout() {
        if (once) {
            if (drawable == null)
                return
            //图片宽高超过屏幕，缩放到适应屏幕的款或者高
            val picW = drawable.intrinsicWidth
            val picH = drawable.intrinsicHeight
            var scale = 1.0f
            if (picH > height && picW <= width) {
                scale = height * 1.0f / picH
                Log.d(tag, "<=>")
            }
            if (picW > width && picH <= height) {
                scale = width * 1.0f / picW
                Log.d(tag, "><=")
            }

            //如果都超出屏幕，按比例适应
            if (picW > width && picH > height) {
                scale = (width * 1.0f / picW).coerceAtMost(height * 1.0f / picH)
                Log.d(tag, ">>")
            }

            if (picW < width && picH < height) {
                scale = min(width * 1.0f / picW, height * 1.0f / picH)
                Log.d(tag, "width$width-picW$picW")
                Log.d(tag, "height$height-picH$picH")
                Log.d(tag, "<<")
            }

            initScale = scale

            //居中显示
            mScaleMatrix.postTranslate((width - picW) / 2f, (height - picH) / 2f)
            //设置缩放
            mScaleMatrix.postScale(scale, scale, width / 2f, height / 2f)

            imageMatrix = mScaleMatrix
            Log.d(tag, "initScale$initScale")
            Log.d(tag, "(width-picW)/2f${(width - picW) / 2f}")
            Log.d(tag, "(height-picH)/2f${(height - picH) / 2f}")
            once = false
        }
    }

    override fun performClick(): Boolean {
        return super.performClick()
    }

    override fun onAttachedToWindow() {
        super.onAttachedToWindow()
        viewTreeObserver.addOnGlobalLayoutListener(this)
    }

    override fun onDetachedFromWindow() {
        super.onDetachedFromWindow()
        viewTreeObserver.removeOnGlobalLayoutListener(this)
    }

    override fun setImageURI(uri: Uri?) {
        super.setImageURI(uri)
        if (visibility != View.VISIBLE)
            visibility = View.VISIBLE
//        onShowImageView()

        if (isShrink) {
            mScaleMatrix = Matrix()
            postDelayed(ShowZoomImageView(onShowImageViewTargetScale()), 16)
            isShrink = false
        }
    }

    override fun setImageBitmap(bm: Bitmap?) {
        super.setImageBitmap(bm)
        if (visibility != View.VISIBLE)
            visibility = View.VISIBLE
//        onShowImageView()

        if (isShrink) {
            mScaleMatrix = Matrix()
            postDelayed(ShowZoomImageView(onShowImageViewTargetScale()), 16)
            isShrink = false
        }
    }

    //获取当前的缩放比例
    private fun getScale(): Float {
        mScaleMatrix.getValues(matrixValues)
        return matrixValues[Matrix.MSCALE_X]
    }

    private fun enlarge(scale: Float, scaleFactor: Float): Boolean {
        //放大的情况，放大倍率不超过最大倍率，且手势是在放大
        return scale < scaleMax && scaleFactor > 1.0f
    }

    private fun enSmall(scale: Float, scaleFactor: Float): Boolean {
        //缩小的情况，缩小倍率不超过最大倍率，且手势是在缩小
        return scale > scaleMin && scaleFactor < 1.0f
    }

    private fun putPicCenterOnScale() {//缩到比初始小，图片会消失，
        val rect = getMatrixRectF()
        var deltaX = 0f
        var deltaY = 0f

        if (rect.width() >= width) {
            if (rect.left > 0) {
                deltaX = -rect.left
            }
            if (rect.right < width) {
                deltaX = width - rect.right
            }
        }

        if (rect.height() >= height) {
            if (rect.top > 0) {
                deltaY = -rect.top
            }
            if (rect.bottom < height) {
                deltaY = height - rect.bottom
            }
        }

        if (rect.width() < width) {
            deltaX = width * 0.5f - rect.right + 0.5f * rect.width()
        }

        if (rect.height() < height) {
            deltaY = height * 0.5f - rect.bottom + 0.5f * rect.height()
        }

        //不知道移动到哪了,移动了几千像素，left和right
        mScaleMatrix.postTranslate(deltaX, deltaY)
//        Log.d(tag, "deltaX$deltaX")
//        Log.d(tag, "deltaY$deltaY")
//        Log.d(tag, "width$width")
//        Log.d(tag, "height$height")
//        Log.d(tag, "rect.width()${rect.width()}")
//        Log.d(tag, "rect.height()${rect.height()}")
//        Log.d(tag, "rect.top${rect.top}")
//        Log.d(tag, "rect.bottom${rect.bottom}")
//        Log.d(tag, "rect.left${rect.left}")
//        Log.d(tag, "rect.right${rect.right}")
    }

    private fun getMatrixRectF(): RectF {
        val matrix = mScaleMatrix
        val rect = RectF()
        if (drawable != null) {
            rect.set(0F, 0F, drawable.intrinsicWidth.toFloat(), drawable.intrinsicHeight.toFloat())
            matrix.mapRect(rect)
        }
        return rect
    }

    /**
     * 移动时，进行边界判断，主要判断宽或高大于屏幕的
     */
    private fun checkMatrixBounds() {
        val rect = getMatrixRectF()

        var deltaX = 0f
        var deltaY = 0f
        val viewWidth = width.toFloat()
        val viewHeight = height.toFloat()
        // 判断移动或缩放后，图片显示是否超出屏幕边界
        if (rect.top > 0 && isCheckTopAndBottom) {
            deltaY = -rect.top
        }
        if (rect.bottom < viewHeight && isCheckTopAndBottom) {
            deltaY = viewHeight - rect.bottom
        }
        if (rect.left > 0 && isCheckLeftAndRight) {
            deltaX = -rect.left
        }
        if (rect.right < viewWidth && isCheckLeftAndRight) {
            deltaX = viewWidth - rect.right
        }
        mScaleMatrix.postTranslate(deltaX, deltaY)
    }

    /**
     * 是否是推动行为
     *
     * @param dx
     * @param dy
     * @return
     */
    private fun isCanDrag(dx: Float, dy: Float): Boolean {
        return sqrt((dx * dx + dy * dy).toDouble()) >= mTouchSlop
    }


    //一帧一帧得缩放bigger，smaller调节缩放速率，因为设置的16ms才做下一帧，肯定不到60fps
    /**
     * 自动缩放的任务
     *
     * @author zhy
     */
    private inner class AutoScaleRunnable
    /**
     * 传入目标缩放值，根据目标值与当前值，判断应该放大还是缩小
     *
     * @param targetScale
     */
    (
            private val mTargetScale: Float,
            /**
             * 缩放的中心
             */
            private val x: Float, private val y: Float
    ) : Runnable {
        private var tmpScale: Float = 0f

        private val bigger = 1.1f //1.07f
        private val smaller = 0.9f//0.93f

        //一帧绘制时间比16ms长了多久，从16里减去，保证下一帧可以满足屏幕刷新率
        private var timeLongerThan16 = 0L

        init {
            tmpScale = if (getScale() < mTargetScale) {
                bigger
            } else {
                smaller
            }

        }

        override fun run() {
            val startTime = System.currentTimeMillis()
            // 进行缩放
            mScaleMatrix.postScale(tmpScale, tmpScale, x, y)
            putPicCenterOnScale()
            imageMatrix = mScaleMatrix

            val currentScale = getScale()
            //如果值在合法范围内，继续缩放
            if (tmpScale > 1f && currentScale < mTargetScale || tmpScale < 1f && mTargetScale < currentScale) {
                this@ZoomImageView.postDelayed(this, 16 - timeLongerThan16)
                Log.d(tag, "16-timeLongerThan16${16 - timeLongerThan16}")
            } else
            //设置为目标的缩放比例
            {
                val deltaScale = mTargetScale / currentScale
                mScaleMatrix.postScale(deltaScale, deltaScale, x, y)
                Log.d(tag, "deltaScale=$deltaScale")

                putPicCenterOnScale()
                imageMatrix = mScaleMatrix
                isAutoScale = false
            }

            timeLongerThan16 = System.currentTimeMillis() - startTime
            Log.d(tag, "run${timeLongerThan16}")
        }

    }

    //隐藏时缩小动画
    private inner class HideZoomImageView : Runnable {
        private var smaller = 0.99f//0.93f
        private var frameCount = 0.1F
        private var count = 1F
        //一帧绘制时间比16ms长了多久，从16里减去，保证下一帧可以满足屏幕刷新率
        private var timeLongerThan16 = 0L

        override fun run() {
            frameCount += 1F / 600 * count
            count += 0.7F
            val startTime = System.currentTimeMillis()
            if (getScale() > 0.01f) {
                //smaller=(1-sin(frameCount*PI/2)).toFloat()
                smaller = (log((frameCount * 1.25), Math.E) / 10.0).toFloat() + 1.0125F
//                Log.d(tag, "frameCount--$frameCount")
                Log.d(tag, "HideZoomImageView smaller--$smaller")

                if (smaller > 1F) {
                    smaller = 0F
                }
                mScaleMatrix.postScale(smaller, smaller, width / 2f, height / 2f)
                imageMatrix = mScaleMatrix

                if(timeLongerThan16>21){
                    visibility = View.GONE
                }else {
                    this@ZoomImageView.postDelayed(this, 16 - timeLongerThan16)
                }//                Log.d(tag, "16-timeLongerThan16--${16 - timeLongerThan16}")
            } else {
                visibility = View.GONE
                isShrink = true
            }
            timeLongerThan16 = System.currentTimeMillis() - startTime
        }
    }

    //显示时放大动画
    private inner class ShowZoomImageView(private val mTargetScale: Float) : Runnable {
        private var bigger = 1.0f //1.07f
        private var frameCount = 0.1F
        //一帧绘制时间比16ms长了多久，从16里减去，保证下一帧可以满足屏幕刷新率
        private var timeLongerThan16 = 0L

        override fun run() {


            val startTime = System.currentTimeMillis()

            //有两种情况
            //1.图片大于屏幕
            //需要放大
            //mTargetScale>1
            //2.图片小于屏幕
            //需要缩小
            //mTargetScale<1

            if (mTargetScale > 1f) {
                //
                if (getScale() < mTargetScale) {
                    frameCount += 1F / 600
                    //bigger=(1-sin(frameCount*PI/2)).toFloat()
                    bigger = (log(((1 - frameCount) * 1.25), Math.E) / 10.0).toFloat() + 1.0125F
//                Log.d(tag, "frameCount--$frameCount")
                    Log.d(tag, "HideZoomImageView bigger--$bigger")

                    if (bigger < 1F) {
                        bigger = 1F
                    }
                    mScaleMatrix.postScale(bigger, bigger, width / 2f, height / 2f)
                    putPicCenterOnScale()
                    imageMatrix = mScaleMatrix


                    this@ZoomImageView.postDelayed(this, 16 - timeLongerThan16)
                    //                Log.d(tag, "16-timeLongerThan16--${16 - timeLongerThan16}")
                }
            } else if (mTargetScale < 1f) {
                mScaleMatrix.postScale(mTargetScale, mTargetScale, width / 2f, height / 2f)
                putPicCenterOnScale()
                imageMatrix = mScaleMatrix
            }
            if (visibility != View.VISIBLE)
                visibility = View.VISIBLE
            timeLongerThan16 = System.currentTimeMillis() - startTime
        }
    }

    private fun onShowImageViewTargetScale(): Float {
        val floatArray = FloatArray(9)
        mScaleMatrix.getValues(floatArray)

        val dw = drawable.bounds.width() * floatArray[0]
        val dh = drawable.bounds.height() * floatArray[4]

        if (!once && width != drawable.intrinsicWidth
                && height != drawable.intrinsicHeight
        ) {
            Log.d(
                    tag, "min(\n" +
                    "                width / dw,\n" +
                    "                height / dh\n" +
                    "            )${min(
                            width / dw,
                            height / dh
                    )}"
            )
            return min(
                    width / dw,
                    height / dh
            )
        }
        return 1.0f
    }

    private fun onShowImageView() {
        mScaleMatrix = Matrix()
        val floatArray = FloatArray(9)
        mScaleMatrix.getValues(floatArray)

        val dw = drawable.bounds.width() * floatArray[0]
        val dh = drawable.bounds.height() * floatArray[4]

        if (!once && width != drawable.intrinsicWidth
                && height != drawable.intrinsicHeight
        ) {
            val scale = min(
                    width / dw,
                    height / dh
            )
            mScaleMatrix.postScale(scale, scale, width / 2f, height / 2f)
            putPicCenterOnScale()
            imageMatrix = mScaleMatrix
        }
    }
}
```
