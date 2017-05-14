这是用来控制第三方播放器的音乐播放，实现播放/暂停、下一曲、上一曲。


```java
//这是播放或暂停
long eventtime = SystemClock.uptimeMillis();

Intent downIntent = new Intent(Intent.ACTION_MEDIA_BUTTON, null);
KeyEvent downEvent = 
    new KeyEvent(eventtime, eventtime, KeyEvent.ACTION_DOWN, KeyEvent.KEYCODE_MEDIA_PLAY_PAUSE, 0);
downIntent.putExtra(Intent.EXTRA_KEY_EVENT, downEvent);
sendOrderedBroadcast(downIntent, null);

Intent upIntent = new Intent(Intent.ACTION_MEDIA_BUTTON, null);
KeyEvent upEvent = new KeyEvent(eventtime, eventtime, KeyEvent.ACTION_UP, KeyEvent.KEYCODE_MEDIA_PLAY_PAUSE, 0);
upIntent.putExtra(Intent.EXTRA_KEY_EVENT, upEvent);
sendOrderedBroadcast(upIntent, null);
```

```java
/*NEXT*/
//这是下一曲
long eventtime = SystemClock.uptimeMillis();
Intent downIntent = new Intent(Intent.ACTION_MEDIA_BUTTON, null);
KeyEvent downEvent = new KeyEvent(eventtime, eventtime, KeyEvent.ACTION_DOWN,   KeyEvent.KEYCODE_MEDIA_NEXT, 0);
downIntent.putExtra(Intent.EXTRA_KEY_EVENT, downEvent);
sendOrderedBroadcast(downIntent, null);
```

```java
/*PREVIOUS*/
//这是上一曲
long eventtime = SystemClock.uptimeMillis();
Intent downIntent = new Intent(Intent.ACTION_MEDIA_BUTTON, null);
KeyEvent downEvent = 
    new KeyEvent(eventtime, eventtime, KeyEvent.ACTION_DOWN, KeyEvent.KEYCODE_MEDIA_PREVIOUS, 0);
downIntent.putExtra(Intent.EXTRA_KEY_EVENT, downEvent);
sendOrderedBroadcast(downIntent, null);
```


以下是另外两种方法，但没有测试成功（这里还可以获取当前是否播放音乐）

第一种：
```java
AudioManager mAudioManager = (AudioManager) getSystemService(Context.AUDIO_SERVICE);

if(mAudioManager.isMusicActive()) {
    Intent i = new Intent(SERVICECMD);
    i.putExtra(CMDNAME , CMDSTOP );
    YourApplicationClass.this.sendBroadcast(i);
}
你可以通过获取audiomanager然后发送命令给它。

这些是命令。

 public static final String CMDTOGGLEPAUSE = "togglepause";
 public static final String CMDPAUSE = "pause";
 public static final String CMDPREVIOUS = "previous";
 public static final String CMDNEXT = "next";
 public static final String SERVICECMD = "com.android.music.musicservicecommand";
 public static final String CMDNAME = "command";
 public static final String CMDSTOP = "stop";
```
第二种：

```
//这是将耳机的中间键发送出去，也可发送其他键
try
{
	Runtime.getRuntime().exec("input keyevent " +Integer.toString(KeyEvent.KEYCODE_HEADSETHOOK));
	//下面代码是长按
	//Runtime.getRuntime().exec("input keyevent --longpress" +
	//						Integer.toString(KeyEvent.KEYCODE_HEADSETHOOK));
}catch (IOException e){}

```
完结