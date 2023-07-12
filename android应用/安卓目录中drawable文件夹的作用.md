A drawable resource is a general concept for a graphic that can be drawn to the screen and which you can retrieve with APIs such as `[getDrawable(int)](https://developer.android.google.cn/reference/android/content/res/Resources#getDrawable(int))` or apply to another XML resource with attributes such as `android:drawable` and `android:icon`. There are several different types of drawables:

<dl style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Bitmap File](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Bitmap)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">A bitmap graphic file (`.png`, `.webp`, `.jpg`, or `.gif`). Creates a `[BitmapDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/BitmapDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Nine-Patch File](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#NinePatch)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">A PNG file with stretchable regions to allow image resizing based on content (`.9.png`). Creates a `[NinePatchDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/NinePatchDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Layer List](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#LayerList)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">A Drawable that manages an array of other Drawables. These are drawn in array order, so the element with the largest index is be drawn on top. Creates a `[LayerDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LayerDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[State List](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#StateList)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that references different bitmap graphics for different states (for example, to use a different image when a button is pressed). Creates a `[StateListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/StateListDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Level List](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#LevelList)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a drawable that manages a number of alternate Drawables, each assigned a maximum numerical value. Creates a `[LevelListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LevelListDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Transition Drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Transition)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a drawable that can cross-fade between two drawable resources. Creates a `[TransitionDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Inset Drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Inset)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a drawable that insets another drawable by a specified distance. This is useful when a View needs a background drawable that is smaller than the View's actual bounds.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Clip Drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Clip)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a drawable that clips another Drawable based on this Drawable's current level value. Creates a `[ClipDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ClipDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Scale Drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Scale)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a drawable that changes the size of another Drawable based on its current level value. Creates a `[ScaleDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ScaleDrawable)`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">[Shape Drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Shape)</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">An XML file that defines a geometric shape, including colors and gradients. Creates a `[GradientDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/GradientDrawable)`.</dd>

</dl>

Also see the [Animation Resource](https://developer.android.google.cn/guide/topics/resources/animation-resource) document for how to create an `[AnimationDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/AnimationDrawable)`.

**Note:** A [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color) can also be used as a drawable in XML. For example, when creating a [state list drawable](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#StateList), you can reference a color resource for the `android:drawable` attribute (`android:drawable="@color/green"`).

## Bitmap

A bitmap image. Android supports bitmap files in the following formats: `.png` (preferred), `,webp` (preferred, requires API level 17 or higher), `.jpg` (acceptable), `.gif` (discouraged).

You can reference a bitmap file directly, using the filename as the resource ID, or create an alias resource ID in XML.

**Note:** Bitmap files may be automatically optimized with lossless image compression by the `aapt` tool during the build process. For example, a true-color PNG that does not require more than 256 colors may be converted to an 8-bit PNG with a color palette. This will result in an image of equal quality but which requires less memory. So be aware that the image binaries placed in this directory can change during the build. If you plan on reading an image as a bit stream in order to convert it to a bitmap, put your images in the `res/raw/` folder instead, where they will not be optimized.

### Bitmap file

A bitmap file is a `.png`, `.webp`, `.jpg`, or `.gif` file. Android creates a `[Drawable](https://developer.android.google.cn/reference/android/graphics/drawable/Drawable)` resource for any of these files when you save them in the `res/drawable/` directory.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.png` (`.png`, `.webp`, `.jpg`, or `.gif`)
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[BitmapDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/BitmapDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">With an image saved at `res/drawable/myimage.png`, this layout XML applies the image to a View: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><ImageView  android:layout_height="wrap_content"  android:layout_width="wrap_content"  android:src="@drawable/myimage"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

The following application code retrieves the image as a `[Drawable](https://developer.android.google.cn/reference/android/graphics/drawable/Drawable)`:

 <devsite-selector scope="auto" active="kotlin" ready="" style="box-sizing: inherit; --devsite-border-radius:8px; --devsite-link-hover:#000; --devsite-tab-marker-color:#80868b; --devsite-selector-margin:32px 0; pointer-events: auto; visibility: visible; background: var(--devsite-selector-background,var(--devsite-background-1)); border: var(--devsite-border,var(--devsite-secondary-border)); border-radius: var(--devsite-border-radius,0); display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ;"><devsite-tabs role="tablist" connected="" style="box-sizing: inherit; --devsite-link-font:var(--android-tab-font); --devsite-link-letter-spacing:var(--android-tab-letter-spacing); --devsite-link-text-transform:none; --devsite-tab-marker-border-radius:3px 3px 0 0; --devsite-tab-marker-height:3px; --devsite-tab-marker-position-x:24px; display: flex; -webkit-box-flex: 1; flex: 1 1 0%; height: var(--devsite-tabs-height,48px); margin: var(--devsite-tabs-margin); max-width: none; position: relative; width: var(--devsite-tabs-width); border-bottom: var(--devsite-border,var(--devsite-secondary-border));"><tab role="tab" aria-selected="true" aria-controls="tabpanel-kotlin" tab="kotlin" id="kotlin" active="" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Kotlin](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#kotlin)</tab><tab role="tab" aria-selected="false" aria-controls="tabpanel-java" tab="java" id="java" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Java](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#java)</tab></devsite-tabs>  <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-kotlin" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px 24px 24px 23px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px); border-radius: var(--devsite-content-border-radius,0);">val drawable:  Drawable?  =  ResourcesCompat.`[getDrawable](https://developer.android.google.cn/reference/androidx/core/content/res/ResourcesCompat#getDrawable(android.content.res.Resources,%20int,%20android.content.res.Resources.Theme))`(resources, R.drawable.myimage,  null)  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:0; --devsite-code-border-radius:0; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: 0px -23px; overflow: hidden; position: relative; direction: ltr !important; --devsite-content-border-radius:0 0 8px 8px;"></devsite-code></devsite-selector> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   [2D Graphics](https://developer.android.google.cn/guide/topics/graphics/2d-graphics)
*   `[BitmapDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/BitmapDrawable)`

</dd>

</dl>

### XML bitmap

An XML bitmap is a resource defined in XML that points to a bitmap file. The effect is an alias for a raw bitmap file. The XML can specify additional properties for the bitmap such as dithering and tiling.

**Note:** You can use a `<bitmap>` element as a child of an `<item>` element. For example, when creating a [state list](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#StateList) or [layer list](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#LayerList), you can exclude the `android:drawable` attribute from an `<item>` element and nest a `<bitmap>` inside it that defines the drawable item.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[BitmapDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/BitmapDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[bitmap](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#bitmap-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:src="@[package:]drawable/*drawable_resource*"  android:antialias=["true" | "false"] android:dither=["true" | "false"] android:filter=["true" | "false"] android:gravity=["top" | "bottom" | "left" | "right" | "center_vertical" | "fill_vertical" | "center_horizontal" | "fill_horizontal" | "center" | "fill" | "clip_vertical" | "clip_horizontal"] android:mipMap=["true" | "false"] android:tileMode=["disabled" | "clamp" | "repeat" | "mirror"] />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="bitmap-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<bitmap>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines the bitmap source and its properties.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`. This is required only if the `<bitmap>` is the root element—it is not needed when the `<bitmap>` is nested inside an `<item>`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:src`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:antialias`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. Enables or disables antialiasing.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:dither`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. Enables or disables dithering of the bitmap if the bitmap does not have the same pixel configuration as the screen (for instance: a ARGB 8888 bitmap with an RGB 565 screen).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:filter`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. Enables or disables bitmap filtering. Filtering is used when the bitmap is shrunk or stretched to smooth its apperance.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:gravity`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. Defines the gravity for the bitmap. The gravity indicates where to position the drawable in its container if the bitmap is smaller than the container.

Must be one or more (separated by '|') of the following constant values:

| Value | Description |
| `top` | Put the object at the top of its container, not changing its size. |
| `bottom` | Put the object at the bottom of its container, not changing its size. |
| `left` | Put the object at the left edge of its container, not changing its size. |
| `right` | Put the object at the right edge of its container, not changing its size. |
| `center_<wbr style="box-sizing: inherit;">vertical` | Place object in the vertical center of its container, not changing its size. |
| `fill_<wbr style="box-sizing: inherit;">vertical` | Grow the vertical size of the object if needed so it completely fills its container. |
| `center_<wbr style="box-sizing: inherit;">horizontal` | Place object in the horizontal center of its container, not changing its size. |
| `fill_<wbr style="box-sizing: inherit;">horizontal` | Grow the horizontal size of the object if needed so it completely fills its container. |
| `center` | Place the object in the center of its container in both the vertical and horizontal axis, not changing its size. |
| `fill` | Grow the horizontal and vertical size of the object if needed so it completely fills its container. This is the default. |
| `clip_<wbr style="box-sizing: inherit;">vertical` | Additional option that can be set to have the top and/or bottom edges of the child clipped to its container's bounds. The clip is based on the vertical gravity: a top gravity clips the bottom edge, a bottom gravity clips the top edge, and neither clips both edges. |
| `clip_<wbr style="box-sizing: inherit;">horizontal` | Additional option that can be set to have the left and/or right edges of the child clipped to its container's bounds. The clip is based on the horizontal gravity: a left gravity clips the right edge, a right gravity clips the left edge, and neither clips both edges. |

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:mipMap`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. Enables or disables the mipmap hint. See `[setHasMipMap()](https://developer.android.google.cn/reference/android/graphics/Bitmap#setHasMipMap(boolean))` for more information. Default value is false.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:tileMode`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. Defines the tile mode. When the tile mode is enabled, the bitmap is repeated. Gravity is ignored when the tile mode is enabled.

Must be one of the following constant values:

| Value | Description |
| `disabled` | Do not tile the bitmap. This is the default value. |
| `clamp` | Replicates the edge color if the shader draws outside of its original bounds |
| `repeat` | Repeats the shader's image horizontally and vertically. |
| `mirror` | Repeats the shader's image horizontally and vertically, alternating mirror images so that adjacent images always seam. |

</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <bitmap  xmlns:android="http://schemas.android.com/apk/res/android"  android:src="@drawable/icon"  android:tileMode="repeat"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[BitmapDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/BitmapDrawable)`
*   [Creating alias resources](https://developer.android.google.cn/guide/topics/resources/providing-resources#AliasResources)

</dd>

</dl>

## Nine-Patch

A `[NinePatch](https://developer.android.google.cn/reference/android/graphics/NinePatch)` is a PNG image in which you can define stretchable regions that Android scales when content within the View exceeds the normal image bounds. You typically assign this type of image as the background of a View that has at least one dimension set to `"wrap_content"`, and when the View grows to accommodate the content, the Nine-Patch image is also scaled to match the size of the View. An example use of a Nine-Patch image is the background used by Android's standard `[Button](https://developer.android.google.cn/reference/android/widget/Button)` widget, which must stretch to accommodate the text (or image) inside the button.

Same as with a normal [bitmap](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#Bitmap), you can reference a Nine-Patch file directly or from a resource defined by XML.

For a complete discussion about how to create a Nine-Patch file with stretchable regions, see the [2D Graphics](https://developer.android.google.cn/guide/topics/graphics/2d-graphics#nine-patch) document.

### Nine-patch file

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 0px 0px 16px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.9.png`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[NinePatchDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/NinePatchDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">With an image saved at `res/drawable/myninepatch.9.png`, this layout XML applies the Nine-Patch to a View: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><Button  android:layout_height="wrap_content"  android:layout_width="wrap_content"  android:background="@drawable/myninepatch"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   [2D Graphics](https://developer.android.google.cn/guide/topics/graphics/2d-graphics#nine-patch)
*   `[NinePatchDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/NinePatchDrawable)`

</dd>

</dl>

### XML Nine-Patch

An XML Nine-Patch is a resource defined in XML that points to a Nine-Patch file. The XML can specify dithering for the image.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[NinePatchDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/NinePatchDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[nine-patch](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#ninepatch-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:src="@[package:]drawable/*drawable_resource*"  android:dither=["true" | "false"] />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="ninepatch-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<nine-patch>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines the Nine-Patch source and its properties.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:src`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a Nine-Patch file.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:dither`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. Enables or disables dithering of the bitmap if the bitmap does not have the same pixel configuration as the screen (for instance: a ARGB 8888 bitmap with an RGB 565 screen).</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <nine-patch  xmlns:android="http://schemas.android.com/apk/res/android"  android:src="@drawable/myninepatch"  android:dither="false"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

</dl>

## Layer list

A `[LayerDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LayerDrawable)` is a drawable object that manages an array of other drawables. Each drawable in the list is drawn in the order of the list—the last drawable in the list is drawn on top.

Each drawable is represented by an `<item>` element inside a single `<layer-list>` element.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[LayerDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LayerDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[layer-list](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#layerlist-element)  xmlns:android="http://schemas.android.com/apk/res/android"  >  <[item](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#layerlist-item-element)  android:drawable="@[package:]drawable/*drawable_resource*"  android:id="@[+][*package*:]id/*resource_name*"  android:top="*dimension*"  android:right="*dimension*"  android:bottom="*dimension*"  android:left="*dimension*"  />  </layer-list>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="layerlist-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<layer-list>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">**Required.** This must be the root element. Contains one or more `<item>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

</dl>

</dd>

<dt id="layerlist-item-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<item>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines a drawable to place in the layer drawable, in a position defined by its attributes. Must be a child of a `<layer-list>` element. Accepts child `<bitmap>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:id`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Resource ID*. A unique resource ID for this drawable. To create a new resource ID for this item, use the form: `"@+id/*name*"`. The plus symbol indicates that this should be created as a new ID. You can use this identifier to retrieve and modify the drawable with `[View.findViewById()](https://developer.android.google.cn/reference/android/view/View#findViewById(int))` or `[Activity.findViewById()](https://developer.android.google.cn/reference/android/app/Activity#findViewById(int))`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:top`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The top offset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:right`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The right offset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:bottom`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The bottom offset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:left`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The left offset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

</dl>

All drawable items are scaled to fit the size of the containing View, by default. Thus, placing your images in a layer list at different positions might increase the size of the View and some images scale as appropriate. To avoid scaling items in the list, use a `<bitmap>` element inside the `<item>` element to specify the drawable and define the gravity to something that does not scale, such as `"center"`. For example, the following `<item>` defines an item that scales to fit its container View:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><item  android:drawable="@drawable/image"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

To avoid scaling, the following example uses a `<bitmap>` element with centered gravity:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><item>  <bitmap  android:src="**@drawable/image**"  android:gravity="center"  />  </item>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">XML file saved at `res/drawable/layers.xml`: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <layer-list  xmlns:android="http://schemas.android.com/apk/res/android">  <item>  <bitmap  android:src="@drawable/android_red"  android:gravity="center"  />  </item>  <item  android:top="10dp"  android:left="10dp">  <bitmap  android:src="@drawable/android_green"  android:gravity="center"  />  </item>  <item  android:top="20dp"  android:left="20dp">  <bitmap  android:src="@drawable/android_blue"  android:gravity="center"  />  </item>  </layer-list>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

Notice that this example uses a nested `<bitmap>` element to define the drawable resource for each item with a "center" gravity. This ensures that none of the images are scaled to fit the size of the container, due to resizing caused by the offset images.

This layout XML applies the drawable to a View:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><ImageView  android:layout_height="wrap_content"  android:layout_width="wrap_content"  android:src="@drawable/layers"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

The result is a stack of increasingly offset images:

[图片上传失败...(image-574c92-1678947221557)]

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[LayerDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LayerDrawable)`

</dd>

</dl>

## State list

A `[StateListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/StateListDrawable)` is a drawable object defined in XML that uses a several different images to represent the same graphic, depending on the state of the object. For example, a `[Button](https://developer.android.google.cn/reference/android/widget/Button)` widget can exist in one of several different states (pressed, focused, or neither) and, using a state list drawable, you can provide a different background image for each state.

You can describe the state list in an XML file. Each graphic is represented by an `<item>` element inside a single `<selector>` element. Each `<item>` uses various attributes to describe the state in which it should be used as the graphic for the drawable.

During each state change, the state list is traversed top to bottom and the first item that matches the current state is used—the selection is *not* based on the "best match," but simply the first item that meets the minimum criteria of the state.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[StateListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/StateListDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[selector](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#selector-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:constantSize=["true" | "false"] android:dither=["true" | "false"] android:variablePadding=["true" | "false"] >  <[item](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#item-element)  android:drawable="@[package:]drawable/*drawable_resource*"  android:state_pressed=["true" | "false"] android:state_focused=["true" | "false"] android:state_hovered=["true" | "false"] android:state_selected=["true" | "false"] android:state_checkable=["true" | "false"] android:state_checked=["true" | "false"] android:state_enabled=["true" | "false"] android:state_activated=["true" | "false"] android:state_window_focused=["true" | "false"] />  </selector>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="selector-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<selector>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">**Required.** This must be the root element. Contains one or more `<item>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:constantSize`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if the drawable's reported internal size remains constant as the state changes (the size is the maximum of all of the states); "false" if the size varies based on the current state. Default is false.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:dither`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" to enable dithering of the bitmap if the bitmap does not have the same pixel configuration as the screen (for instance, an ARGB 8888 bitmap with an RGB 565 screen); "false" to disable dithering. Default is true.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:variablePadding`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if the drawable's padding should change based on the current state that is selected; "false" if the padding should stay the same (based on the maximum padding of all the states). Enabling this feature requires that you deal with performing layout when the state changes, which is often not supported. Default is false.</dd>

</dl>

</dd>

<dt id="item-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<item>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines a drawable to use during certain states, as described by its attributes. Must be a child of a `<selector>` element.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_pressed`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is pressed (such as when a button is touched/clicked); "false" if this item should be used in the default, non-pressed state.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_focused`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object has input focus (such as when the user selects a text input); "false" if this item should be used in the default, non-focused state.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_hovered`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is being hovered by a cursor; "false" if this item should be used in the default, non-hovered state. Often, this drawable may be the same drawable used for the "focused" state.

Introduced in API level 14.

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_selected`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is the current user selection when navigating with a directional control (such as when navigating through a list with a d-pad); "false" if this item should be used when the object is not selected.

The selected state is used when focus (`android:state_focused`) is not sufficient (such as when list view has focus and an item within it is selected with a d-pad).

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_checkable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is checkable; "false" if this item should be used when the object is not checkable. (Only useful if the object can transition between a checkable and non-checkable widget.)</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_checked`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is checked; "false" if it should be used when the object is un-checked.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_enabled`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is enabled (capable of receiving touch/click events); "false" if it should be used when the object is disabled.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_activated`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the object is activated as the persistent selection (such as to "highlight" the previously selected list item in a persistent navigation view); "false" if it should be used when the object is not activated.

Introduced in API level 11.

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:state_window_focused`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this item should be used when the application window has focus (the application is in the foreground), "false" if this item should be used when the application window does not have focus (for example, if the notification shade is pulled down or a dialog appears).</dd>

</dl>

**Note:** Remember that Android applies the first item in the state list that matches the current state of the object. So, if the first item in the list contains none of the state attributes above, then it is applied every time, which is why your default value should always be last (as demonstrated in the following example).

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">XML file saved at `res/drawable/button.xml`: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <selector  xmlns:android="http://schemas.android.com/apk/res/android">  <item  android:state_pressed="true"  android:drawable="@drawable/button_pressed"  />  <!-- pressed -->  <item  android:state_focused="true"  android:drawable="@drawable/button_focused"  />  <!-- focused -->  <item  android:state_hovered="true"  android:drawable="@drawable/button_focused"  />  <!-- hovered -->  <item  android:drawable="@drawable/button_normal"  />  <!-- default -->  </selector>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

This layout XML applies the state list drawable to a Button:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><Button  android:layout_height="wrap_content"  android:layout_width="wrap_content"  android:background="@drawable/button"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[StateListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/StateListDrawable)`

</dd>

</dl>

## Level list

A Drawable that manages a number of alternate Drawables, each assigned a maximum numerical value. Setting the level value of the drawable with `[setLevel()](https://developer.android.google.cn/reference/android/graphics/drawable/Drawable#setLevel(int))` loads the drawable resource in the level list that has a `android:maxLevel` value greater than or equal to the value passed to the method.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[LevelListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LevelListDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[level-list](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#levellist-element)  xmlns:android="http://schemas.android.com/apk/res/android"  >  <[item](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#levellist-item-element)  android:drawable="@drawable/*drawable_resource*"  android:maxLevel="*integer*"  android:minLevel="*integer*"  />  </level-list>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="levellist-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<level-list>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">This must be the root element. Contains one or more `<item>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

</dl>

</dd>

<dt id="levellist-item-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<item>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines a drawable to use at a certain level.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource to be inset.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:maxLevel`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The maximum level allowed for this item.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:minLevel`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The minimum level allowed for this item.</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <level-list  xmlns:android="http://schemas.android.com/apk/res/android"  >  <item  android:drawable="@drawable/status_off"  android:maxLevel="0"  />  <item  android:drawable="@drawable/status_on"  android:maxLevel="1"  />  </level-list>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: ; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

Once this is applied to a `[View](https://developer.android.google.cn/reference/android/view/View)`, the level can be changed with `[setLevel()](https://developer.android.google.cn/reference/android/graphics/drawable/Drawable#setLevel(int))` or `[setImageLevel()](https://developer.android.google.cn/reference/android/widget/ImageView#setImageLevel(int))`.

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[LevelListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LevelListDrawable)`

</dd>

</dl>

## Transition drawable

A `[TransitionDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable)` is a drawable object that can cross-fade between the two drawable resources.

Each drawable is represented by an `<item>` element inside a single `<transition>` element. No more than two items are supported. To transition forward, call `[startTransition()](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable#startTransition(int))`. To transition backward, call `[reverseTransition()](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable#reverseTransition(int))`.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[TransitionDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[transition](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#transition-element)  xmlns:android="http://schemas.android.com/apk/res/android"  >  <[item](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#transition-item-element)  android:drawable="@[package:]drawable/*drawable_resource*"  android:id="@[+][*package*:]id/*resource_name*"  android:top="*dimension*"  android:right="*dimension*"  android:bottom="*dimension*"  android:left="*dimension*"  />  </transition>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="transition-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<transition>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">**Required.** This must be the root element. Contains one or more `<item>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

</dl>

</dd>

<dt id="transition-item-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<item>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines a drawable to use as part of the drawable transition. Must be a child of a `<transition>` element. Accepts child `<bitmap>` elements.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:id`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Resource ID*. A unique resource ID for this drawable. To create a new resource ID for this item, use the form: `"@+id/*name*"`. The plus symbol indicates that this should be created as a new ID. You can use this identifier to retrieve and modify the drawable with `[View.findViewById()](https://developer.android.google.cn/reference/android/view/View#findViewById(int))` or `[Activity.findViewById()](https://developer.android.google.cn/reference/android/app/Activity#findViewById(int))`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:top`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The top offset in pixels.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:right`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The right offset in pixels.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:bottom`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The bottom offset in pixels.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:left`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The left offset in pixels.</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">XML file saved at `res/drawable/transition.xml`: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <transition  xmlns:android="http://schemas.android.com/apk/res/android">  <item  android:drawable="@drawable/on"  />  <item  android:drawable="@drawable/off"  />  </transition>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

This layout XML applies the drawable to a View:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><ImageButton  android:id="@+id/button"  android:layout_height="wrap_content"  android:layout_width="wrap_content"  android:src="@drawable/transition"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

And the following code performs a 500ms transition from the first item to the second:

 <devsite-selector scope="auto" active="kotlin" ready="" style="box-sizing: inherit; --devsite-border-radius:8px; --devsite-link-hover:#000; --devsite-tab-marker-color:#80868b; --devsite-selector-margin:32px 0; pointer-events: auto; visibility: visible; background: var(--devsite-selector-background,var(--devsite-background-1)); border: var(--devsite-border,var(--devsite-secondary-border)); border-radius: var(--devsite-border-radius,0); display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ;"><devsite-tabs role="tablist" connected="" style="box-sizing: inherit; --devsite-link-font:var(--android-tab-font); --devsite-link-letter-spacing:var(--android-tab-letter-spacing); --devsite-link-text-transform:none; --devsite-tab-marker-border-radius:3px 3px 0 0; --devsite-tab-marker-height:3px; --devsite-tab-marker-position-x:24px; display: flex; -webkit-box-flex: 1; flex: 1 1 0%; height: var(--devsite-tabs-height,48px); margin: var(--devsite-tabs-margin); max-width: none; position: relative; width: var(--devsite-tabs-width); border-bottom: var(--devsite-border,var(--devsite-secondary-border));"><tab role="tab" aria-selected="true" aria-controls="tabpanel-kotlin" tab="kotlin" id="kotlin" active="" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Kotlin](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#kotlin)</tab><tab role="tab" aria-selected="false" aria-controls="tabpanel-java" tab="java" id="java" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Java](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#java)</tab></devsite-tabs>  <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-kotlin" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px 24px 24px 23px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px); border-radius: var(--devsite-content-border-radius,0);">val button:  ImageButton  = findViewById(R.id.button)  val drawable:  Drawable  = button.drawable if  (drawable is  TransitionDrawable)  { drawable.startTransition(500)  }  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:0; --devsite-code-border-radius:0; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: 0px -23px; overflow: hidden; position: relative; direction: ltr !important; --devsite-content-border-radius:0 0 8px 8px;"></devsite-code></devsite-selector> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[TransitionDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/TransitionDrawable)`

</dd>

</dl>

## Inset drawable

A drawable defined in XML that insets another drawable by a specified distance. This is useful when a View needs a background that is smaller than the View's actual bounds.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[InsetDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/InsetDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[inset](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#inset-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/*drawable_resource*"  android:insetTop="*dimension*"  android:insetRight="*dimension*"  android:insetBottom="*dimension*"  android:insetLeft="*dimension*"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="inset-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<inset>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines the inset drawable. This must be the root element.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource to be inset.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:insetTop`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The top inset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension)</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:insetRight`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The right inset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension)</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:insetBottom`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The bottom inset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension)</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:insetLeft`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The left inset, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension)</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <inset  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/background"  android:insetTop="10dp"  android:insetLeft="10dp"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[InsetDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/InsetDrawable)`

</dd>

</dl>

## Clip drawable

A drawable defined in XML that clips another drawable based on this Drawable's current level. You can control how much the child drawable gets clipped in width and height based on the level, as well as a gravity to control where it is placed in its overall container. Most often used to implement things like progress bars.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[ClipDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ClipDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[clip](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#clip-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/*drawable_resource*"  android:clipOrientation=["horizontal" | "vertical"] android:gravity=["top" | "bottom" | "left" | "right" | "center_vertical" |"fill_vertical" | "center_horizontal" | "fill_horizontal" |"center" | "fill" | "clip_vertical" | "clip_horizontal"] />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="clip-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<clip>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines the clip drawable. This must be the root element.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource to be clipped.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:clipOrientation`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. The orientation for the clip.

Must be one of the following constant values:

| Value | Description |
| `horizontal` | Clip the drawable horizontally. |
| `vertical` | Clip the drawable vertically. |

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:gravity`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. Specifies where to clip within the drawable.

Must be one or more (separated by '|') of the following constant values:

| Value | Description |
| `top` | Put the object at the top of its container, not changing its size. When `clipOrientation` is `"vertical"`, clipping occurs at the bottom of the drawable. |
| `bottom` | Put the object at the bottom of its container, not changing its size. When `clipOrientation` is `"vertical"`, clipping occurs at the top of the drawable. |
| `left` | Put the object at the left edge of its container, not changing its size. This is the default. When `clipOrientation` is `"horizontal"`, clipping occurs at the right side of the drawable. This is the default. |
| `right` | Put the object at the right edge of its container, not changing its size. When `clipOrientation` is `"horizontal"`, clipping occurs at the left side of the drawable. |
| `center_<wbr style="box-sizing: inherit;">vertical` | Place object in the vertical center of its container, not changing its size. Clipping behaves the same as when gravity is `"center"`. |
| `fill_<wbr style="box-sizing: inherit;">vertical` | Grow the vertical size of the object if needed so it completely fills its container. When `clipOrientation` is `"vertical"`, no clipping occurs because the drawable fills the vertical space (unless the drawable level is 0, in which case it's not visible). |
| `center_<wbr style="box-sizing: inherit;">horizontal` | Place object in the horizontal center of its container, not changing its size. Clipping behaves the same as when gravity is `"center"`. |
| `fill_<wbr style="box-sizing: inherit;">horizontal` | Grow the horizontal size of the object if needed so it completely fills its container. When `clipOrientation` is `"horizontal"`, no clipping occurs because the drawable fills the horizontal space (unless the drawable level is 0, in which case it's not visible). |
| `center` | Place the object in the center of its container in both the vertical and horizontal axis, not changing its size. When `clipOrientation` is `"horizontal"`, clipping occurs on the left and right. When `clipOrientation` is `"vertical"`, clipping occurs on the top and bottom. |
| `fill` | Grow the horizontal and vertical size of the object if needed so it completely fills its container. No clipping occurs because the drawable fills the horizontal and vertical space (unless the drawable level is 0, in which case it's not visible). |
| `clip_<wbr style="box-sizing: inherit;">vertical` | Additional option that can be set to have the top and/or bottom edges of the child clipped to its container's bounds. The clip is based on the vertical gravity: a top gravity clips the bottom edge, a bottom gravity clips the top edge, and neither clips both edges. |
| `clip_<wbr style="box-sizing: inherit;">horizontal` | Additional option that can be set to have the left and/or right edges of the child clipped to its container's bounds. The clip is based on the horizontal gravity: a left gravity clips the right edge, a right gravity clips the left edge, and neither clips both edges. |

</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">XML file saved at `res/drawable/clip.xml`: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <clip  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/android"  android:clipOrientation="horizontal"  android:gravity="left"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

The following layout XML applies the clip drawable to a View:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><ImageView  android:id="@+id/image"  android:src="@drawable/clip"  android:layout_height="wrap_content"  android:layout_width="wrap_content"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

The following code gets the drawable and increases the amount of clipping in order to progressively reveal the image:

 <devsite-selector scope="auto" active="kotlin" ready="" style="box-sizing: inherit; --devsite-border-radius:8px; --devsite-link-hover:#000; --devsite-tab-marker-color:#80868b; --devsite-selector-margin:32px 0; pointer-events: auto; visibility: visible; background: var(--devsite-selector-background,var(--devsite-background-1)); border: var(--devsite-border,var(--devsite-secondary-border)); border-radius: var(--devsite-border-radius,0); display: block; margin: var(--devsite-selector-margin,16px 0);"><devsite-tabs role="tablist" connected="" style="box-sizing: inherit; --devsite-link-font:var(--android-tab-font); --devsite-link-letter-spacing:var(--android-tab-letter-spacing); --devsite-link-text-transform:none; --devsite-tab-marker-border-radius:3px 3px 0 0; --devsite-tab-marker-height:3px; --devsite-tab-marker-position-x:24px; display: flex; -webkit-box-flex: 1; flex: 1 1 0%; height: var(--devsite-tabs-height,48px); margin: var(--devsite-tabs-margin); max-width: none; position: relative; width: var(--devsite-tabs-width); border-bottom: var(--devsite-border,var(--devsite-secondary-border));"><tab role="tab" aria-selected="true" aria-controls="tabpanel-kotlin" tab="kotlin" id="kotlin" active="" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Kotlin](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#kotlin)</tab><tab role="tab" aria-selected="false" aria-controls="tabpanel-java" tab="java" id="java" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Java](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#java)</tab></devsite-tabs>  <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-kotlin" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px 24px 24px 23px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px); border-radius: var(--devsite-content-border-radius,0);">val imageview:  ImageView  = findViewById(R.id.image)  val drawable:  Drawable  = imageview.background if  (drawable is  ClipDrawable)  { drawable.level = drawable.level +  1000  }  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:0; --devsite-code-border-radius:0; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: 0px -23px; overflow: hidden; position: relative; direction: ltr !important; --devsite-content-border-radius:0 0 8px 8px;"></devsite-code></devsite-selector> 

Increasing the level reduces the amount of clipping and slowly reveals the image. Here it is at a level of 7000:

[图片上传失败...(image-9a465-1678947221557)]

**Note:** The default level is 0, which is fully clipped so the image is not visible. When the level is 10,000, the image is not clipped and completely visible.

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[ClipDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ClipDrawable)`

</dd>

</dl>

## Scale drawable

A drawable defined in XML that changes the size of another drawable based on its current level.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[ScaleDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ScaleDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[scale](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#scale-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/*drawable_resource*"  android:scaleGravity=["top" | "bottom" | "left" | "right" | "center_vertical" | "fill_vertical" | "center_horizontal" | "fill_horizontal" | "center" | "fill" | "clip_vertical" | "clip_horizontal"] android:scaleHeight="*percentage*"  android:scaleWidth="*percentage*"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="scale-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<scale>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Defines the scale drawable. This must be the root element.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:drawable`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Drawable resource*. **Required**. Reference to a drawable resource.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:scaleGravity`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. Specifies the gravity position after scaling.

Must be one or more (separated by '|') of the following constant values:

| Value | Description |
| `top` | Put the object at the top of its container, not changing its size. |
| `bottom` | Put the object at the bottom of its container, not changing its size. |
| `left` | Put the object at the left edge of its container, not changing its size. This is the default. |
| `right` | Put the object at the right edge of its container, not changing its size. |
| `center_<wbr style="box-sizing: inherit;">vertical` | Place object in the vertical center of its container, not changing its size. |
| `fill_<wbr style="box-sizing: inherit;">vertical` | Grow the vertical size of the object if needed so it completely fills its container. |
| `center_<wbr style="box-sizing: inherit;">horizontal` | Place object in the horizontal center of its container, not changing its size. |
| `fill_<wbr style="box-sizing: inherit;">horizontal` | Grow the horizontal size of the object if needed so it completely fills its container. |
| `center` | Place the object in the center of its container in both the vertical and horizontal axis, not changing its size. |
| `fill` | Grow the horizontal and vertical size of the object if needed so it completely fills its container. |
| `clip_<wbr style="box-sizing: inherit;">vertical` | Additional option that can be set to have the top and/or bottom edges of the child clipped to its container's bounds. The clip is based on the vertical gravity: a top gravity clips the bottom edge, a bottom gravity clips the top edge, and neither clips both edges. |
| `clip_<wbr style="box-sizing: inherit;">horizontal` | Additional option that can be set to have the left and/or right edges of the child clipped to its container's bounds. The clip is based on the horizontal gravity: a left gravity clips the right edge, a right gravity clips the left edge, and neither clips both edges. |

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:scaleHeight`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Percentage*. The scale height, expressed as a percentage of the drawable's bound. The value's format is XX%. For instance: 100%, 12.5%, etc.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:scaleWidth`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Percentage*. The scale width, expressed as a percentage of the drawable's bound. The value's format is XX%. For instance: 100%, 12.5%, etc.</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <scale  xmlns:android="http://schemas.android.com/apk/res/android"  android:drawable="@drawable/logo"  android:scaleGravity="center_vertical|center_horizontal"  android:scaleHeight="80%"  android:scaleWidth="80%"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[ScaleDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ScaleDrawable)`

</dd>

</dl>

## Shape drawable

This is a generic shape defined in XML.

<dl class="xml" style="box-sizing: inherit; margin: 0px; padding: 0px; color: rgb(32, 33, 36); font-family: Roboto, &quot;Noto Sans&quot;, &quot;Noto Sans JP&quot;, &quot;Noto Sans KR&quot;, &quot;Noto Naskh Arabic&quot;, &quot;Noto Sans Thai&quot;, &quot;Noto Sans Hebrew&quot;, &quot;Noto Sans Bengali&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">file location:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">`res/drawable/*filename*.xml`
The filename is used as the resource ID.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">compiled resource datatype:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Resource pointer to a `[GradientDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/GradientDrawable)`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">resource reference:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">In Java: `R.drawable.*filename*`
In XML: `@[*package*:]drawable/*filename*`</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">syntax:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;"> <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <[shape](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#shape-element)  xmlns:android="http://schemas.android.com/apk/res/android"  android:shape=["rectangle" | "oval" | "line" | "ring"] >  <[corners](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#corners-element)  android:radius="*integer*"  android:topLeftRadius="*integer*"  android:topRightRadius="*integer*"  android:bottomLeftRadius="*integer*"  android:bottomRightRadius="*integer*"  />  <[gradient](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#gradient-element)  android:angle="*integer*"  android:centerX="*float*"  android:centerY="*float*"  android:centerColor="*integer*"  android:endColor="*color*"  android:gradientRadius="*integer*"  android:startColor="*color*"  android:type=["linear" | "radial" | "sweep"] android:useLevel=["true" | "false"] />  <[padding](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#padding-element)  android:left="*integer*"  android:top="*integer*"  android:right="*integer*"  android:bottom="*integer*"  />  <[size](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#size-element)  android:width="*integer*"  android:height="*integer*"  />  <[solid](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#solid-element)  android:color="*color*"  />  <[stroke](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#stroke-element)  android:width="*integer*"  android:color="*color*"  android:dashWidth="*integer*"  android:dashGap="*integer*"  />  </shape>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin-top: 0px; margin-right: ; margin-bottom: 0px; margin-left: ; overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">elements:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

<dl class="tag-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt id="shape-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<shape>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">The shape drawable. This must be the root element.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`xmlns:android`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*String*. **Required.** Defines the XML namespace, which must be `"http://schemas.android.com/apk/res/android"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:shape`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. Defines the type of shape. Valid values are:

| Value | Desciption |
| `"rectangle"` | A rectangle that fills the containing View. This is the default shape. |
| `"oval"` | An oval shape that fits the dimensions of the containing View. |
| `"line"` | A horizontal line that spans the width of the containing View. This shape requires the `<stroke>` element to define the width of the line. |
| `"ring"` | A ring shape. |

</dd>

</dl>

The following attributes are used only when `android:shape="ring"`:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:innerRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for the inner part of the ring (the hole in the middle), as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:innerRadiusRatio`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Float*. The radius for the inner part of the ring, expressed as a ratio of the ring's width. For instance, if `android:innerRadiusRatio="5"`, then the inner radius equals the ring's width divided by 5\. This value is overridden by `android:innerRadius`. Default value is 9.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:thickness`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The thickness of the ring, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:thicknessRatio`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Float*. The thickness of the ring, expressed as a ratio of the ring's width. For instance, if `android:thicknessRatio="2"`, then the thickness equals the ring's width divided by 2\. This value is overridden by `android:innerRadius`. Default value is 3.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:useLevel`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this is used as a `[LevelListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LevelListDrawable)`. This should normally be "false" or your shape may not appear.</dd>

</dl>

</dd>

<dt id="corners-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<corners>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Creates rounded corners for the shape. Applies only when the shape is a rectangle.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:radius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for all corners, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension). This is overridden for each corner by the following attributes.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:topLeftRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for the top-left corner, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:topRightRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for the top-right corner, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:bottomLeftRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for the bottom-left corner, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:bottomRightRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The radius for the bottom-right corner, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

</dl>

**Note:** Every corner must (initially) be provided a corner radius greater than 1, or else no corners are rounded. If you want specific corners to *not* be rounded, a work-around is to use `android:radius` to set a default corner radius greater than 1, but then override each and every corner with the values you really want, providing zero ("0dp") where you don't want rounded corners.

</dd>

<dt id="gradient-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<gradient>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Specifies a gradient color for the shape.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:angle`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Integer*. The angle for the gradient, in degrees. 0 is left to right, 90 is bottom to top. It must be a multiple of 45\. Default is 0.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:centerX`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Float*. The relative X-position for the center of the gradient (0 - 1.0).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:centerY`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Float*. The relative Y-position for the center of the gradient (0 - 1.0).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:centerColor`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Color*. Optional color that comes between the start and end colors, as a hexadecimal value or [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:endColor`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Color*. The ending color, as a hexadecimal value or [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:gradientRadius`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Float*. The radius for the gradient. Only applied when `android:type="radial"`.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:startColor`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Color*. The starting color, as a hexadecimal value or [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:type`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Keyword*. The type of gradient pattern to apply. Valid values are:

| Value | Description |
| `"linear"` | A linear gradient. This is the default. |
| `"radial"` | A radial gradient. The start color is the center color. |
| `"sweep"` | A sweeping line gradient. |

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:useLevel`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Boolean*. "true" if this is used as a `[LevelListDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/LevelListDrawable)`.</dd>

</dl>

</dd>

<dt id="padding-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<padding>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">Padding to apply to the containing View element (this pads the position of the View content, not the shape).

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:left`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. Left padding, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:top`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. Top padding, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:right`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. Right padding, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:bottom`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. Bottom padding, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

</dl>

</dd>

<dt id="size-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<size>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">The size of the shape.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:height`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The height of the shape, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:width`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The width of the shape, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

</dl>

**Note:** The shape scales to the size of the container View proportionate to the dimensions defined here, by default. When you use the shape in an `[ImageView](https://developer.android.google.cn/reference/android/widget/ImageView)`, you can restrict scaling by setting the [`android:scaleType`](https://developer.android.google.cn/reference/android/widget/ImageView#attr_android:scaleType) to `"center"`.

</dd>

<dt id="solid-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<solid>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">A solid color to fill the shape.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:color`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Color*. The color to apply to the shape, as a hexadecimal value or [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color).</dd>

</dl>

</dd>

<dt id="stroke-element" style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`<stroke>`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">A stroke line for the shape.

attributes:

<dl class="atn-list" style="box-sizing: inherit; margin: 0px; padding: 0px;">

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:width`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The thickness of the line, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:color`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Color*. The color of the line, as a hexadecimal value or [color resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Color).</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:dashGap`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The distance between line dashes, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension). Only valid if `android:dashWidth` is set.</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">`android:dashWidth`</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">*Dimension*. The size of each dash line, as a dimension value or [dimension resource](https://developer.android.google.cn/guide/topics/resources/more-resources#Dimension). Only valid if `android:dashGap` is set.</dd>

</dl>

</dd>

</dl>

</dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">example:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">XML file saved at `res/drawable/gradient_box.xml`: <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><?xml version="1.0" encoding="utf-8"?>  <shape  xmlns:android="http://schemas.android.com/apk/res/android"  android:shape="rectangle">  <gradient  android:startColor="#FFFF0000"  android:endColor="#80FF00FF"  android:angle="45"/>  <padding  android:left="7dp"  android:top="7dp"  android:right="7dp"  android:bottom="7dp"  />  <corners  android:radius="8dp"  />  </shape>  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

This layout XML applies the shape drawable to a View:

 <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-xml" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px);"><TextView  android:background="@drawable/gradient_box"  android:layout_height="wrap_content"  android:layout_width="wrap_content"  />  </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:var(--devsite-secondary-border); --devsite-code-border-radius:8px; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: var(--devsite-code-margin,16px 0); overflow: hidden; position: relative; direction: ltr !important;"></devsite-code> 

This application code gets the shape drawable and applies it to a View:

 <devsite-selector scope="auto" active="kotlin" ready="" style="box-sizing: inherit; --devsite-border-radius:8px; --devsite-link-hover:#000; --devsite-tab-marker-color:#80868b; --devsite-selector-margin:32px 0; pointer-events: auto; visibility: visible; background: var(--devsite-selector-background,var(--devsite-background-1)); border: var(--devsite-border,var(--devsite-secondary-border)); border-radius: var(--devsite-border-radius,0); display: block; margin-top: ; margin-right: ; margin-bottom: 0px; margin-left: ;"><devsite-tabs role="tablist" connected="" style="box-sizing: inherit; --devsite-link-font:var(--android-tab-font); --devsite-link-letter-spacing:var(--android-tab-letter-spacing); --devsite-link-text-transform:none; --devsite-tab-marker-border-radius:3px 3px 0 0; --devsite-tab-marker-height:3px; --devsite-tab-marker-position-x:24px; display: flex; -webkit-box-flex: 1; flex: 1 1 0%; height: var(--devsite-tabs-height,48px); margin: var(--devsite-tabs-margin); max-width: none; position: relative; width: var(--devsite-tabs-width); border-bottom: var(--devsite-border,var(--devsite-secondary-border));"><tab role="tab" aria-selected="true" aria-controls="tabpanel-kotlin" tab="kotlin" id="kotlin" active="" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Kotlin](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#kotlin)</tab><tab role="tab" aria-selected="false" aria-controls="tabpanel-java" tab="java" id="java" style="box-sizing: inherit; display: flex; flex-shrink: 0; position: relative;">[Java](https://developer.android.google.cn/guide/topics/resources/drawable-resource?hl=en#java)</tab></devsite-tabs>  <devsite-code data-copy-event-label="" style="box-sizing: inherit; --devsite-code-background:#f8f9fa; --devsite-code-color:#37474f; --devsite-var-color:#d01884; --devsite-code-buttons-toggle-dark-display:inline; --devsite-code-buttons-toggle-light-display:none; --devsite-code-comments-color:#b80672; --devsite-code-keywords-color:#1967d2; --devsite-code-numbers-color:#c5221f; --devsite-code-strings-color:#188038; --devsite-code-types-color:#9334e6; --devsite-github-link-icon:url(&quot;data:image/svg+xml;utf8,<svg xmlns=\&quot;http://www.w3.org/2000/svg\&quot; viewBox=\&quot;0 0 18 18\&quot;><pre class="lang-kotlin" translate="no" dir="ltr" is-upgraded="" style="box-sizing: inherit; background: var(--devsite-code-background); color: var(--devsite-code-color); font: 14px/20px var(--devsite-code-font-family); padding: 24px 24px 24px 23px; direction: ltr !important; text-align: left !important; margin: 0px; overflow-x: auto; position: relative; padding-block: var(--devsite-code-padding-block,24px); padding-inline: var(--devsite-code-padding-inline,24px); border-radius: var(--devsite-content-border-radius,0);">val shape:  Drawable?  =  `[getDrawable](https://developer.android.google.cn/reference/androidx/core/content/res/ResourcesCompat#getDrawable(android.content.res.Resources,%20int,%20android.content.res.Resources.Theme))`(`[resources](https://developer.android.google.cn/reference/android/content/Context#getResources())`, R.drawable.gradient_box,  `[getTheme()](https://developer.android.google.cn/reference/android/content/Context#getTheme())`)  val tv:  TextView  = findViewById(R.id.textview) tv.background = shape </pre><path d=\&quot;M9-.06A9,9,0,0,0,6.16,17.48c.44.09.59-.19.59-.43V15.38c-2.5.54-3-1.07-3-1.07a2.35,2.35,0,0,0-1-1.31c-.82-.56.06-.55.06-.55a1.89,1.89,0,0,1,1.38.93,1.92,1.92,0,0,0,2.62.75,1.91,1.91,0,0,1,.57-1.21c-2-.23-4.1-1-4.1-4.45a3.49,3.49,0,0,1,.92-2.41,3.25,3.25,0,0,1,.09-2.38S5,3.43,6.75,4.6a8.59,8.59,0,0,1,4.5,0c1.72-1.17,2.48-.92,2.48-.92a3.25,3.25,0,0,1,.09,2.38,3.49,3.49,0,0,1,.92,2.41c0,3.46-2.1,4.22-4.11,4.44a2.14,2.14,0,0,1,.62,1.67v2.47c0,.24.14.52.6.43A9,9,0,0,0,9-.06Z\&quot; fill=\&quot;%231a73e8\&quot;/></svg>&quot;); --devsite-code-border:0; --devsite-code-border-radius:0; --devsite-code-margin:32px 0; border: var(--devsite-code-border,0); border-radius: var(--devsite-code-border-radius,0); clear: both; display: block; margin: 0px -23px; overflow: hidden; position: relative; direction: ltr !important; --devsite-content-border-radius:0 0 8px 8px;"></devsite-code></devsite-selector> </dd>

<dt style="box-sizing: inherit; font: 700 16px/24px var(--devsite-primary-font-family); margin: 16px 0px;">see also:</dt>

<dd style="box-sizing: inherit; margin: 16px 0px; padding: 0px 0px 0px 40px;">

*   `[ShapeDrawable](https://developer.android.google.cn/reference/android/graphics/drawable/ShapeDrawable)`

</dd>

</dl>
