---
title: ImagaSource
---

This class encapsulates the common abstraction over a platform-specific (Bitmap for Android and UIImage for iOS) image objects.
<!-- TODO: add links -->
<!-- TODO: add Preview -->
## Using ImageSource


### Loading an image using a resource name

To load an image from the [App_Resources]() folders, use the [fromResource()](#fromresource) or [fromResourceSync()](#fromresourcesync) method:

```ts
ImageSource.fromResource('logo')
  .then((image: ImageSource) => {
    console.log(image.width)
  })
  .catch(err => {
    console.error(err.stack)
  })
```

### Loading an image from the device file system

To load an image from a file, use any of [fromFile()](#fromfile), [fromFileOrResourceSync()](#fromfileorresourcesync) or the [fromFileSync()] method:

```ts
async function loadImage(){

try {

const imageFromFile: ImageSource = await ImageSource.fromFile(filePath)

} catch(error) {

    }
}
```

### Creating an image from a base64 string

To create an image from a base64 string, use the [fromBase64()](#tobase64string) or [fromBase64Sync()](#frombase64sync):

```ts
const base64Str = "some base64Str"
const image: ImageSource = ImageSource.fromBase64Sync(base64Str)

```

### Saving an image to a file

To save an ImageSource instance to a file, call the [saveToFile()](#savetofile) method on the instance.

```ts
async function saveImage(){

try {
    
    const folderDest: Folder = knownFolders.documents()

    folderDest.getFile('/images/test.png') //1. Create the file

    const pathDest = path.join(folderDest.path, '/images/test.png')
    const saved: boolean = await image.saveToFileAsync(pathDest, 'png') // Save to the file
    if (saved) {
      Dialogs.alert('Saved successfully')
    }
  } catch (err) {
    Dialogs.alert(err)
  }

}
```

## ImageSource API

### constructor
```ts
const imageSource = new ImageSource(nativeSource)
```
 Creates a new ImageSource instance and sets the provided native source object. `nativeSource` object will update either the android or ios properties, depending on the target os.

---
The ImageSource class provides the following image static methods loaders.

### fromAsset()

```ts
ImageSource.fromAsset(asset: ImageAsset)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```

Loads an ImageSource instance from the specified ImageAsset instance asynchronously.

---
### fromBase64()
```ts
ImageSource.fromBase64(base64String)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```
Loads an ImageSource instance from the specified base64 encoded string asynchronously

---

### fromBase64Sync()
```ts
const imageSource: ImageSource = ImageSource.fromBase64Sync(base64String)
```
Loads an ImageSource instance from the specified base64 encoded string synchronously.

---
### fromData()
```ts
ImageSource.fromData(data)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```
Loads an ImageSource instance from the specified native image data(byte array) asynchronously. `data` can be a Stream on Android and [NSData](https://developer.apple.com/documentation/foundation/nsdata) on iOS.

---
### fromDataSync()

```ts
const imageSource: ImageSource = ImageSource.fromDataSync(data);
```

Loads an ImageSource instance from the specified native image data(byte array) synchronously.

---

### fromFile()
```ts
ImageSource.fromFile(path)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```

Loads an ImageSource instance from the specified file asynchronously.

---
### fromFileSync()
```ts
const imageSource: ImageSource = ImageSource.fromFileSync(data);
```

Loads an ImageSource instance from the specified file synchronously.

### fromFileOrResourceSync()
```ts
const imageSource: ImageSource = ImageSource.fromFileOrResourceSync(path);
```

Create an ImageSource from the specified local file or resource (if specified with the `"res://"` prefix).

---

### fromFontIconCodeSync()
```ts
const imageSource: ImageSource = ImageSource.fromFontIconCodeSync(source, font, color);
```

Creates a new ImageSource instance from the specified font icon code.

---
### fromResource()
```ts
ImageSource.fromResource(name)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```
Creates an ImageSource from the specified resource name(without the extension) asynchronously.

---
### fromResourceSync()

```ts
const imageSource: ImageSource = ImageSource.fromResourceSync(name)
```

Creates an ImageSource from the specified resource name(without the extension) synchronously.

---
### fromUrl()

```ts
ImageSource.fromUrl(url)
.then((imageSource: ImageSource) =>{
// handle the created image
})
.catch(error =>{
    // handle errror
})
```

Downloads and decodes the image from the provided Url and creates a new ImageSource instance from it.

---
The following is the API for the loaded ImageSource instance.

### android
```ts
const imageAndroid: android.graphics.BitMap = imageSource.android
```
The Android-specific [Bitmap](http://developer.android.com/reference/android/graphics/Bitmap.html) instance.

---

### ios
```ts
const imageIOS: UIImage = imageSource.ios
```
 The iOS-specific [UIImage](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/) instance.

---

### height
```ts
const height: number = imageSource.height
```
Gets the height of the instance.

---
### width
```ts
const width: number = imageSource.width
```
Gets the width of the instance.

---
### rotationAngle
```ts
const rotationAngle: number = imageSource.rotationAngle
```

`Android-only`: Gets or sets the rotation angle that should be applied to the image.

---
### resizeAsync()
```ts
imageSource.resize(maxSize, options)
.then((resizedImage: ImageSource)=>{

}).
catch(error=>{

})
```

Asynchronously returns a new ImageSource that is a resized version of the imageSource with the same aspect ratio, and the max dimension set to the provided maxSize.

- `maxSize` is the maximum desired pixel dimension of the resulting image.
- _Optional_: (`Android` only) `options.filter` options.filter is a `boolean` which determines whether or not bilinear filtering should be used when scaling the bitmap. If `true` then bilinear filtering will be used when scaling which has better image quality at the cost of worse performance. If this is false then nearest-neighbor scaling is used instead which will have worse image quality but is faster. Recommended is to set filter to 'true' as the cost of bilinear filtering is typically minimal and the improved image quality is significant.

---
### resize()

```ts
const resizedImage: ImageSource = imageSource.resize(maxSize, options)
```

Returns a new ImageSource that is a resized version of the imageSource with the same aspect ratio, and the max dimension set to the provided `maxSize`.

---

### saveToFile()
```ts
const isSaved: boolean = imageSource.saveToFile(path, format, quality)
```

Saves this instance to the specified file, using the provided image `format` and `quality`.

---
### saveToFileAsync()

```ts
imageSource.saveToFileAsync(path, format, quality)
.then((isSaved:boolean)=>{

})
.catch(error=>{

})
```

Asynchronously saves this instance to the specified file, using the provided image `format` and `quality`.

- `path` (`string`) is the path of the file on the file system to save to.
- `format` (`'png' | 'jpeg' | 'jpg'`) is the format (encoding) of the image.
- _Optional_: `quality` specifies the quality of the encoding. It defaults to the maximum available quality, and varies on a scale of 0 to 100.

---
### setNativeSource()
```ts
imageSource.setNativeSource(nativeSource)
```
Sets the provided native source object, either a Bitmap for Android or a UIImage for iOS.

---
### toBase64String()

```ts
const base64String : string = imageSource.toBase64String(format, quality)
```

Converts the image to base64 encoded string, using the provided image format and quality.

---
### toBase64StringAsync()

```ts
imageSource.toBase64StringAsync(format, quality)
            .then((base64String:string)=>{

            })
            .catch(error=>{

            })
```

Asynchronously converts the image to base64 encoded string, using the provided image format and quality.


## API Reference(s)
- [ImageSource](https://docs.nativescript.org/api-reference/classes/imagesource) class

## Native Component

- `android`: [android.graphics.Bitmap](http://developer.android.com/reference/android/graphics/Bitmap.html)
- `iOS`: [UIImage](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class)
