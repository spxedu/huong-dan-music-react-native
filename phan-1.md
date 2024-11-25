## Bước 1: Cài đặt thư viện
```
npm install --save react-native-track-player
```
## Bước 2: Tạo file service chạy nhạc trackPlayerServices.js
Trong code dưới đây: 
- TrackPlayer.setupPlayer: Dùng để khởi động một đối tượng quản lý chơi nhạc
- TrackPlayer.updateOptions: Dùng để cài đặt các thiết lập
- addTracks: Hàm dùng khai báo danh sách các bài hát
- playbackService: Hàm dùng khai báo các sự kiện tương tác với Player

 
```
import TrackPlayer, {
  AppKilledPlaybackBehavior,
  Capability,
  RepeatMode,
  Event
} from 'react-native-track-player';
export async function setupPlayer() {
  let isSetup = false;
  try {
    await TrackPlayer.getCurrentTrack();
    isSetup = true;
  }
  catch {
    await TrackPlayer.setupPlayer();
    await TrackPlayer.updateOptions({
      android: {
        appKilledPlaybackBehavior:
          AppKilledPlaybackBehavior.StopPlaybackAndRemoveNotification,
      },
      capabilities: [
        Capability.Play,
        Capability.Pause,
        Capability.SkipToNext,
        Capability.SkipToPrevious,
        Capability.SeekTo,
      ],
      compactCapabilities: [
        Capability.Play,
        Capability.Pause,
        Capability.SkipToNext,
      ],
      progressUpdateEventInterval: 2,
    });
    isSetup = true;
  }
  finally {
    return isSetup;
  }
}
export async function addTracks() {
  await TrackPlayer.add([
    {
      id: '1',
      url: 'https://cdn.pixabay.com/audio/2022/10/18/audio_31c2730e64.mp3',
      title: 'Password Infinity',
      artist: 'zezo.dev',
     }
  ]);
  await TrackPlayer.setRepeatMode(RepeatMode.Queue);
}
export async function playbackService() {
  // Hàm này để sau khai báo các event điều khiển
}
```

## Bước 3: Khai báo trong file index.js
 
```
import TrackPlayer from 'react-native-track-player';
import { playbackService } from './trackPlayerServices';
AppRegistry.registerComponent(appName, () => App);
TrackPlayer.registerPlaybackService(() => playbackService);
```

## Bước 4: Code trong file App.tsx

```
// 1. import các thư viện
import React, { useEffect, useState } from 'react';
import {
  SafeAreaView,
  StyleSheet,
  View,
  Button,
  ActivityIndicator,
} from 'react-native';
import TrackPlayer from 'react-native-track-player';
import { setupPlayer, addTracks } from './trackPlayerServices';


function App() {
// khai báo trạng thái để kiểm soát nếu chưa sẵn sàng load nhạc thì hiện thị activityIndicator quay quay
  const [isPlayerReady, setIsPlayerReady] = useState(false);
  // khi render thì bắt đầu khởi tạo đối tượng chơi nhạc
  
  useEffect(() => {
    async function setup() {
      let isSetup = await setupPlayer();
      const queue = await TrackPlayer.getQueue();
      if(isSetup && queue.length <= 0) {
        await addTracks();
      }
      setIsPlayerReady(isSetup);
    }
    setup();
  }, []);
  
  
  
  if(!isPlayerReady) {
    return (
      <SafeAreaView style={styles.container}>
        <ActivityIndicator size="large" color="#bbb"/>
      </SafeAreaView>
    );
  }
  
  
  return (
    <SafeAreaView style={styles.container}>
      <Button title="Play" color="#777" onPress={() => TrackPlayer.play()}/>
    </SafeAreaView>
  );
}
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    padding: 20,
    backgroundColor: '#112'
  },
});
export default App;
```

## Bước 5: Chạy thử ứng dụng
Ứng dụng ở bước này sẽ có một nút play ở màn hình, bấm nút sẽ bắt đầu phát nhạc, ứng dụng sẽ chạy lặp lại bài hát vĩnh viễn nhờ lệnh setRepeatMode ở trong hàm addTracks() trong file service.
