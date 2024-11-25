Trước khi thao tác theo bài viết này, bạn cần có ứng dụng đơn giản theo hướng dẫn ở bài viết trước https://github.com/spxedu/huong-dan-music-react-native/blob/main/phan-2.md

Thư viện hỗ trợ các hàm giúp bạn điều khiển quá trình chơi nhạc

- TrackPlayer.pause: Tạm dừng chơi nhạc, bạn có thể tiếp tục bằng hàm TrackPlayer.play
- TrackPlayer.seekTo: Chuyển tới một vị trí nào đó trên chuỗi thời gian của bài nhạc (thường gọi là tua)
- TrackPlayer.setRate: Thay đổi tốc độ chơi nhạc
- TrackPlayer.setVolume: Thay đổi âm lượng của bản nhạc

Để điều khiển các thành phần trên thường phải lấy trạng thái hiện tại

- TrackPlayer.getState: Lấy trạng thái bản nhạc hiện tại đã lưu
- TrackPlayer.getVolume: Lấy cấp độ âm lượng hiện tại đang thiết lập
Ví dụ cách thiết lập bằng các hàm

```
TrackPlayer.pause()
TrackPlayer.seekTo(10)
TrackPlayer.setRate(0.5)
TrackPlayer.setVolume(0.2)
```

## Bước 1: Bạn cài thư viện react-native-vector-icons để làm biểu tượng nút bấm
Bạn xem trong bài viết hướng dẫn cài thư viện này

https://github.com/spxedu/huong-dan-music-react-native/blob/main/huong-dan-react-native-vector-icon.md
 

## Bước 2: Thêm import thư viện vào file App.tsx

```javascript
// cái nào có rồi thì bạn lược bỏ đi
import TrackPlayer ,{
  useTrackPlayerEvents,
  usePlaybackState,
  Event,
  State
} from 'react-native-track-player';
import Icon from 'react-native-vector-icons/FontAwesome';
```
## Bước 3: Thêm component Controls để hiển thị nút điều khiển
```javascript
function Controls({ onShuffle }) {
  const playerState = usePlaybackState();
  async function handlePlayPress() {
    if(await TrackPlayer.getState() == State.Playing) {
      TrackPlayer.pause();
    }
    else {
      TrackPlayer.play();
    }
  }
  return(
    <View style={{flexDirection: 'row',
      flexWrap: 'wrap', alignItems: 'center'}}>
        <Icon.Button
          name="arrow-left"
          size={28}
          backgroundColor="transparent"
          color="red"
          onPress={() => TrackPlayer.skipToPrevious()}/>
        <Icon.Button
          name={playerState.state == State.Playing ? 'pause' : 'play'}
          size={28}
          backgroundColor="transparent"
          color="blue"
          onPress={handlePlayPress}/>
        <Icon.Button
          name="arrow-right"
          size={28}
          backgroundColor="transparent"
          color="red"
          onPress={() => TrackPlayer.skipToNext()}/>
    </View>
  );
}
```

## Bước 4: Chỉnh sửa component Playlist như sau
 
```javascript
function Playlist() {
  const [queue, setQueue] = useState([]);
  const [currentTrack, setCurrentTrack] = useState(0);
  async function loadPlaylist() {
    const queue = await TrackPlayer.getQueue();
    setQueue(queue);
  }
  useEffect(() => {
    loadPlaylist();
  }, []);
  useTrackPlayerEvents([Event.PlaybackTrackChanged], (event) => {
    if(event.state == State.nextTrack) {
      TrackPlayer.getCurrentTrack().then((index) => setCurrentTrack(index));
    }
  });
  function PlaylistItem({index, title, isCurrent}) {
    function handleItemPress() {
      TrackPlayer.skip(index);
    }
    return (
      <TouchableOpacity onPress={handleItemPress}>
        <Text
          style={{...styles.playlistItem,
            ...{backgroundColor: isCurrent ? '#666' : 'transparent'}}}>
        {title}
        </Text>
      </TouchableOpacity>
    );
  }
  return(
    <View>
      <View style={styles.playlist}>
        <FlatList
          data={queue}
          renderItem={({item, index}) => <PlaylistItem
                                            index={index}
                                            title={item.title}
                                            isCurrent={currentTrack == index }/>
          }
        />
      </View>
      <Controls/>
    </View>
  );
} 
```
## Bước 5: Xóa nút bấm ở phần return của component chính
