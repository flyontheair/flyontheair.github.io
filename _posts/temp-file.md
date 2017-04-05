#### java.awt.image.BufferedImage 类
提供一个供访问的图片缓冲区
方法:
```
isAlphaPremultiplied()
Returns whether or not the alpha has been premultiplied.
```
返回

属性：`TRANSLUCENT`、`TYPE_INT_RGB`


#### javax.imageio.imageio 类
1、`public static BufferedImage read(InputStream input) throws IOException`
```java
public static BufferedImage read(InputStream input)
                          throws IOException
Returns a BufferedImage as the result of decoding a supplied InputStream with an ImageReader chosen automatically from among those currently registered. The InputStream is wrapped in an ImageInputStream. If no registered ImageReader claims to be able to read the resulting stream, null is returned.
The current cache settings from getUseCacheand getCacheDirectory will be used to control caching in the ImageInputStream that is created.

This method does not attempt to locate ImageReaders that can read directly from an InputStream; that may be accomplished using IIORegistry and ImageReaderSpi.

This method does not close the provided InputStream after the read operation has completed; it is the responsibility of the caller to close the stream, if desired.
```
从InputStream流中读取图片信息到BufferedImage，方法不会自己关闭输入流，需要调用者自己关闭
