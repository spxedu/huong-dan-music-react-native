# Hướng dẫn làm ứng dụng chơi nhạc có các chức năng điều khiển trong ReactNative - Phần 5
Để thực hiện được phần này bạn cần có ứng dụng đã chạy được ở phần 2 hoặc phần 4

#### Phần này sẽ thực hiện cài đặt các event điều khiển trong service

#### Bạn vào file trackPlayerServices.js để viết code cho hàm playbackService

```javascript
export async function playbackService() {
  // Các điều khiển dưới đây khai báo xong bạn có thể điều khiển ở phần notify trên điện thoại
  TrackPlayer.addEventListener(Event.RemotePause, () => {
    console.log('Event.RemotePause');
    TrackPlayer.pause();
  });
  TrackPlayer.addEventListener(Event.RemotePlay, () => {
    console.log('Event.RemotePlay');
    TrackPlayer.play();
  });
  TrackPlayer.addEventListener(Event.RemoteNext, () => {
    console.log('Event.RemoteNext');
    TrackPlayer.skipToNext();
  });
  TrackPlayer.addEventListener(Event.RemotePrevious, () => {
    console.log('Event.RemotePrevious');
    TrackPlayer.skipToPrevious();
  });
}
```

![Service music](https://raw.githubusercontent.com/spxedu/huong-dan-music-react-native/refs/heads/main/service.png)
 
Chú ý: Trong android khi ứng dụng để lâu sẽ bị kill để giải phóng tài nguyên cho điện thoại, khi đó bạn có thể lựa chọn phương án cho chạy tiếp hoặc dừng chạy nhạc. Bạn thiết lập trong hàm setupPlayer của file service. 

```javascript
  await TrackPlayer.updateOptions({
      android: {
        appKilledPlaybackBehavior:
          AppKilledPlaybackBehavior.StopPlaybackAndRemoveNotification, // dừng chạy nhạc và loại bỏ notify
          // AppKilledPlaybackBehavior.ContinuePlayback, // tiếp tục chạy đến hết bài nhạc.

      }, 
```
 

 

 

 

 
