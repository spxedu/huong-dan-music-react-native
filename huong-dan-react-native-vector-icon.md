Hướng dẫn cài đặt thư viện react-native-vector-icons nhúng biểu tượng vào ứng dụng ReactNative

Link thư viện https://github.com/oblador/react-native-vector-icons 

## Chạy lệnh cài đặt thư viện
```
npm install --save react-native-vector-icons
``` 

## Cài đặt riêng dành cho chạy trên điện thoại android
Bạn vào sửa file ** android/app/build.gradle **  (file này ở cấp độ module), chú ý không phải file ở android/build.gradle

Bạn thêm phần code dưới đây vào cuối file
```
project.ext.vectoricons = [
    iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf', 'FontAwesome.ttf' ] // Specify font files
]

apply from: file("../../node_modules/react-native-vector-icons/fonts.gradle")
```

Phần dành cho các nền tảng khác sẽ cập nhật sau

## Ví dụ sử dụng đơn giản
```javascript
//1. Nhúng thư viện
import Icon from 'react-native-vector-icons/FontAwesome';

// Ví dụ 1: 
const myButton = (
  <Icon.Button
    name="facebook"
    backgroundColor="#3b5998"
    onPress={this.loginWithFacebook}
  >
    Login with Facebook
  </Icon.Button>
);



// ví dụ 2: 
const customTextButton = (
  <Icon.Button name="facebook" backgroundColor="#3b5998">
    <Text style={{ fontFamily: 'Arial', fontSize: 15 }}>
      Login with Facebook
    </Text>
  </Icon.Button>
);

```

## Các thuộc tính cơ bản

 ![Các thuộc tính cơ bản](https://raw.githubusercontent.com/spxedu/huong-dan-music-react-native/refs/heads/main/icon.jpg)
 
 
 

