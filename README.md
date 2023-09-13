# Getting Started
Add the dependency to your project `build.gradle` file:

<pre><code class="lang-groovy">dependencies {
    ...
    implementation("com.airbnb.android:lottie-compose:$lottieVersion")
    ...
}
</code></pre>

Create : https://github.com/airbnb/lottie/blob/master/android-compose.md

# Example

Sử dụng `rememberLottieComposition` để lấy một composition(một biến lưu trữ biểu đồ animation của Lottie, ở đây là file demo.json) từ một tài nguyên Lottie.
```kotlin
    val composition by rememberLottieComposition(LottieCompositionSpec.RawRes(R.raw.demo))
```

Đây là một biến `isPlaying` được định nghĩa là một `mutableStateOf`, nó có thể thay đổi giá trị và sẽ giúp ta tạo lại Composable khi giá trị thay đổi.
Biến này sẽ kiểm soát trạng thái của animation.
```kotlin
    var isPlaying by remember { mutableStateOf(true) }
```

Sử dụng `animateLottieCompositionAsState` để theo dõi tiến độ của animation dựa trên composition và trạng thái của biến isPlaying.
`progress` là giá trị tiến trình chạy của animation.
```kotlin
    val progress by animateLottieCompositionAsState(composition = composition, isPlaying = isPlaying)
```

Tạo 1 `Composable function` để thực hiện các tác vụ một lần khi một giá trị thay đổi. Nó sẽ hoạt động khi biến `progress` thay đổi
```kotlin
    LaunchedEffect(key1 = progress){
        if (progress == 0f){
            isPlaying = true
        }
        if (progress == 1f){
            isPlaying = false
        }
    }
```

Hiển thị animation Lottie
```kotlin
    LottieAnimation(
        modifier = Modifier.size(300.dp), composition = composition, progress = {
            progress
        })
```

### Code Overview
```kotlin
    @Composable
fun MainScreen() {
    val composition by rememberLottieComposition(LottieCompositionSpec.RawRes(R.raw.demo))
    var isPlaying by remember { mutableStateOf(true) }
    val progress by animateLottieCompositionAsState(
        composition = composition,
        isPlaying = isPlaying
    )

    LaunchedEffect(key1 = progress) {
        if (progress == 0f) {
            isPlaying = true
        }
        if (progress == 1f) {
            isPlaying = false
        }
    }
    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        LottieAnimation(
            modifier = Modifier.size(300.dp), composition = composition, progress = {
                progress
            })
        Button(onClick = { isPlaying = true }) {
            Text(text = "Play")
        }
    }
}
```

# Video Demo

![](video.mp4)