# Hướng dẫn làm ứng dụng chơi nhạc có các chức năng điều khiển trong ReactNative - Phần 4

Trước khi thao tác theo bài viết này, bạn cần có ứng dụng theo hướng dẫn ở bài viết trước
Thư viện có hỗ trợ sử dụng hàm hook useProgress để giám sát tiến trình chạy nhạc.

## Bước 1: Tạo một component hiển thị tiến trình
Component này sẽ hiển thị tiến trình theo định dạng phút:giây (mm:ss)

```javascript
function TrackProgress() {
  const { position, duration } = useProgress(200);
  function format(seconds) {
    let mins = (parseInt(seconds / 60)).toString().padStart(2, '0');
    let secs = (Math.trunc(seconds) % 60).toString().padStart(2, '0');
    return `${mins}:${secs}`;
  }
  return(
    <View>
      <Text style={styles.trackProgress}>
        { format(position) } / { format(duration) }
      </Text>
    </View>
  );
}
```

## Bước 2: Tạo một component hiển thị thông tin về bài nhạc

```javascript
function Header() {
  const [info, setInfo] = useState({});
  useEffect(() => {
    setTrackInfo();
  }, []);
  useTrackPlayerEvents([Event.PlaybackTrackChanged], (event) => {
    if(event.state == State.nextTrack) {
      setTrackInfo();
    }
  });
  async function setTrackInfo() {
    const track = await TrackPlayer.getCurrentTrack();
    const info = await TrackPlayer.getTrack(track);
    setInfo(info);
  }
  return(
    <View>
        <Text style={styles.songTitle}>{info.title}</Text>
        <Text style={styles.artistName}>{info.artist}</Text>
    </View>
  );
}
```

## Bước 3: Thêm lệnh nhúng thư viện vào đầu file App.tsx
(thành phần nào đã có thì bạn bỏ qua không nhúng nữa)
```javascript
import TrackPlayer ,{
  useTrackPlayerEvents,
  usePlaybackState,
  useProgress,
  Event,
  State
} from 'react-native-track-player';
```

## Bước 4: Sửa phần return của component App để hiển thị header và progress

```javascript
return (
  <SafeAreaView style={styles.container}>
    <Header/>
    <TrackProgress/>
    <Playlist/>
  </SafeAreaView>
);
```
## Bước 5: Thêm style cho phần header và progress
```javascript
  trackProgress: {
    marginTop: 40,
    textAlign: 'center',
    fontSize: 24,
    color: '#eee'
  },
  songTitle: {
    fontSize: 32,
    marginTop: 50,
    color: '#ccc'
  },
  artistName: {
    fontSize: 24,
    color: '#888'
  },
```
## Bước 6: Hiển thị thanh slide tiến trình
- Bạn cài thư viện 
- Bạn nhúng thư viện vào đầu file chứa component  TrackProgress 
https://github.com/callstack/react-native-slider 
```javascript
import Slider from '@react-native-community/slider';
```
- Bạn thêm vào phần return của TrackProgress thẻ Slider (có thể sửa style của thẻ view chứa để căn chỉnh cho đẹp

```javascript
  return(
    <View style={{alignItems:'center'}}>
      <Text style={styles.trackProgress}>
        { format(position) } / { format(duration) }
      </Text>
      <Slider
          style={{width: 200, height: 40}}
          minimumValue={0}
          maximumValue={duration}
          minimumTrackTintColor="#FFFFFF"
          maximumTrackTintColor="#000000"
          value={position}
        />
    </View>
  );
```
Bước 6: Chạy ứng dụng và thử nghiệm kết quả

![Chạy nhạc có hiện tiến trình](https://raw.githubusercontent.com/spxedu/huong-dan-music-react-native/refs/heads/main/progress.png)
