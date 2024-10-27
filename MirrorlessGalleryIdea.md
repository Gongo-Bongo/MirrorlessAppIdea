## MirrorLess Raw View App Idea

### Folder Tree

#### Navigation Host:
In the app package make a package named **navigationHost**. Then inside the package creat a kotlin file called `NavigationHost.kt` and write the following content:
```kotlin
import androidx.compose.runtime.Composable
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController
import com.example.morrorlessrawgallery.Screens.HomeScreen

@Composable
fun Navigation() {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = "home_screen") {
        composable("home_screen") { HomeScreen(navController) }
//        composable("detail_screen") { DetailScreen(navController) }
    }
}
```

#### Screens
Creat a `Screen` package in the the app package and creat the screens here. I have created the home screen as follow:

```kotlin
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.runtime.Composable
import androidx.navigation.NavController

import com.example.morrorlessrawgallery.widgets.Folders

@Composable
fun HomeScreen(navController: NavController) {
    val itemId = "123"
    LazyColumn {
        item { // Wrap the Row in 'item' scope
            Row(horizontalArrangement = Arrangement.SpaceEvenly) {
                Folders(
                    folderImage = "https://github.com/Somu6969/Dashami-Scooty/raw/refs/heads/main/DSC05968.ARW.jpg",
                    folderName = "Dashami And Scooty",
                    folderDescription = "Roaming at Navami"
                )
                Folders(
                    folderImage = "",
                    folderName = "Gym",
                    folderDescription = "Making body at AzadHind Club"
                )

            }
            Row(horizontalArrangement = Arrangement.SpaceEvenly) {
                Folders(
                    folderImage = "https://github.com/Somu6969/Dashami-Scooty/raw/refs/heads/main/DSC05968.ARW.jpg",
                    folderName = "Dashami And Scooty",
                    folderDescription = "Roaming at Navami"
                )
                Folders(
                    folderImage = "",
                    folderName = "Gym",
                    folderDescription = "Making body at AzadHind Club"
                )

            }
            Row(horizontalArrangement = Arrangement.SpaceEvenly) {
                Folders(
                    folderImage = "https://github.com/Somu6969/Dashami-Scooty/raw/refs/heads/main/DSC05968.ARW.jpg",
                    folderName = "Dashami And Scooty",
                    folderDescription = "Roaming at Navami"
                )
                Folders(
                    folderImage = "",
                    folderName = "Gym",
                    folderDescription = "Making body at AzadHind Club"
                )

            }
            Row(horizontalArrangement = Arrangement.SpaceEvenly) {
                Folders(
                    folderImage = "https://github.com/Somu6969/Dashami-Scooty/raw/refs/heads/main/DSC05968.ARW.jpg",
                    folderName = "Dashami And Scooty",
                    folderDescription = "Roaming at Navami"
                )
                Folders(
                    folderImage = "",
                    folderName = "Gym",
                    folderDescription = "Making body at AzadHind Club"
                )

            }
        }
    }
}
```

Feel free to modify the above and add more animations and other things to it.\
Creat more screens in this package to make things organised.

#### widgets
Creat another package named `widgets` and then creat a kotlin file named `FolderCard.kt` and write the following content:

```kotlin
import androidx.compose.animation.AnimatedVisibility
import androidx.compose.animation.core.Animatable
import androidx.compose.animation.core.tween
import androidx.compose.animation.fadeIn
import androidx.compose.animation.fadeOut
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.gestures.detectTapGestures
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.layout.width
import androidx.compose.foundation.layout.wrapContentHeight
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Card
import androidx.compose.material3.CardDefaults
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.graphicsLayer
import androidx.compose.ui.input.pointer.pointerInput
import androidx.compose.ui.text.style.TextOverflow
import androidx.compose.ui.unit.dp
import coil3.compose.rememberAsyncImagePainter

@Composable
fun Folders(folderImage: String, folderName: String, folderDescription: String) {
    val cardElevation = remember { Animatable(4f) } // For scaling animation
    val isPressed = remember { mutableStateOf(false) }

    // Animated visibility and load for image
    val imagePainter = rememberAsyncImagePainter(
        model = folderImage,
        onLoading = { isPressed.value = true },
        onSuccess = { isPressed.value = false }
    )

    Card(
        modifier = Modifier
            .padding(12.dp)
            .wrapContentHeight()
            .width(156.dp)
            .pointerInput(Unit) {
                detectTapGestures(
                    onPress = {
                        cardElevation.animateTo(cardElevation.value) // Scale effect on press
                        tryAwaitRelease()
                        cardElevation.animateTo(cardElevation.value) // Return to normal
                    }
                )
            }
            .graphicsLayer { scaleX = 1.02f; scaleY = 1.02f },
        elevation = CardDefaults.cardElevation(8.dp),
        shape = RoundedCornerShape(16.dp),
        colors = CardDefaults.cardColors(MaterialTheme.colorScheme.onSurface)
    ) {
        Column(
            modifier = Modifier
                .padding(12.dp)
                .fillMaxWidth(),
            horizontalAlignment = Alignment.Start,
        ) {
            AnimatedVisibility(visible = true) {
                Image(
                    painter = imagePainter,
                    contentDescription = null,
                    modifier = Modifier
                        .size(120.dp)
                        .clip(RoundedCornerShape(12.dp))
                        .background(MaterialTheme.colorScheme.onSurface.copy(alpha = 0.1f)) // Subtle background if image fails
                        .animateEnterExit(
                            enter = fadeIn(
                                initialAlpha = 0f,
                                animationSpec = tween(durationMillis = 500)
                            ),
                            exit = fadeOut(animationSpec = tween(durationMillis = 300))
                        )
                )
            }

            Spacer(modifier = Modifier.height(8.dp))

            Text(
                text = folderName,
                style = MaterialTheme.typography.labelLarge,
                color = MaterialTheme.colorScheme.onPrimary,
//                maxLines = 1,
                overflow = TextOverflow.Ellipsis
            )

            Spacer(modifier = Modifier.height(4.dp))

            Text(
                text = folderDescription,
                style = MaterialTheme.typography.labelSmall,
                color = MaterialTheme.colorScheme.onPrimary.copy(alpha = 0.7f),
//                maxLines = 2,
                overflow = TextOverflow.Ellipsis
            )
        }
    }
}

```
Creat more widgets in this folder to make things organised.

#### `MainActivity.kt`:
Write the following inside `MianActivity.kt`:
```kotlin
import android.annotation.SuppressLint
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Search
import androidx.compose.material.icons.filled.Settings
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.material3.TopAppBar
import androidx.compose.material3.TopAppBarDefaults
import androidx.compose.runtime.Composable
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.mirrorlessrawgallery.ui.theme.MirrorlessRawGalleryTheme
import com.example.morrorlessrawgallery.navigationHost.Navigation
//import com.example.morrorlessrawgallery.ui.theme.MorrorLessRawGalleryTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            MyApp()

        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@SuppressLint("UnusedMaterial3ScaffoldPaddingParameter")
@Composable
fun MyApp() {
    val settingsEnabled = remember {
        mutableStateOf(false)
    }
    MirrorlessRawGalleryTheme {
        Scaffold(
            modifier = Modifier.fillMaxSize(),
            topBar = {
                TopAppBar(
                    title = {
                        Text(
                            text = "Gallery",
                            style = MaterialTheme.typography.headlineSmall,
                            color = Color.White,
                            modifier = Modifier.padding(start = 8.dp)
                        )
                    },
                    colors = TopAppBarDefaults.topAppBarColors(MaterialTheme.colorScheme.primary),
                    actions = {
                        IconButton(onClick = { /* Handle search action */ }) {
                            Icon(
                                imageVector = Icons.Default.Search,
                                contentDescription = "Search",
                                tint = Color.White
                            )
                        }
                        IconButton(onClick = {

                        }) {
                            Icon(
                                imageVector = Icons.Default.Settings,
                                contentDescription = "Settings",
                                tint = MaterialTheme.colorScheme.onPrimary
                            )
                        }
                    }
                )
            }
        )
        { innerPadding ->
            Column(modifier = Modifier.padding(innerPadding)) {

                Navigation()
            }


        }
    }
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    MyApp()

}
```
You are good to go! Make awesome UI and have fun!