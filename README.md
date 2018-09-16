
## TMDbMovies

**TMDbMovies** is a simple iOS project that fetches movies from TMDb API and renders them using `UICollectionView`.  The project does not use any third-party libraries and is built using all native components.

## Features

 - Enter text to search for movies from TMDb
 - Infinite scrolling with lots of movies
 - Faster scrolling (using basic image caching)
 - Cached recent searches

## Screenshots

<div id="imagesMain">

  <img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/HomeScreen.png" width=30% hspace="5">

  <img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/SearchSuggestions.png" width=30% hspace="5">

  <img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/SearchResults.png" width=30% hspace="5">

</div>



## Architecture and Modules

**TMDbMovies** follows basic MVVM design pattern. It consists of Models, Views, ViewModels, and ViewControllers. Here communication from ViewModels to View is done through Callbacks *(Reactive pattern is recommended)*. MVVM(combined with Dependency injection) are used in the project to make Unit Testing efficient since it has priority in requirements.

Since *pictures are worth a thousand words*, attaching here a few images which shows the class sequence diagram of most of the functionality. Hope that saves your time while reviewing the code

`TMMoviesListViewController` which is the home screen with search and listing feature has two view models. One for the `UICollectionView` to list Movies and other for `UITableView` for recent search suggestions. Below here is class sequence diagram of both of them

#### MVVM
<img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/Classes%20communication%20between%20ViewController%20and%20View%20Model%20copy.png" width=100% title="Class sequence showing communication between classses from UIViewController to ViewModel"/>
<img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/Classes%20communication%20between%20ViewController%20and%20View%20Model%20(1).png" width=100% title="Class sequence showing communication between classses from UIViewController to ViewModel"/>

#### Networking and Service
<img src="https://github.com/vinayakparmar92/TMDbMovies/blob/master/Classes%20communication%20between%20ViewModel%20and%20NetworkLayer%20copy.png" width=100% title="Class sequence showing communication between classses from ViewModel to Network Layer
"/>

#### Image Caching
Image Caching has been implemented using `NSCache`.  The cache gets cleared manually(in `didReceiveMemoryWarning`) when the app receives a Memory Warning. Also the cache is cleared by OS when app is killed due to the behavior of `NSCache`. `URLCache` is an altnerate option that can be used if more control is required.

#### Unit tests
Test cases are written for important functionalities. Name and description of all test cases are as follows:

• `testMoviesInfoOfflineJSONDecoding()`

• `testMoviesListOfflineJSONDecoding()`

• `testInvalidTokenOfflineModelDecoding()`

• `testInvalidPageNumberForMovieListSearch()`
	
> Above are offline tests to check if response models decoding is working fine. Codable is used to map JSON data to models

• `testLoadMoreMovieListFunctionalityFromOfflineData()`

> An offline testcase to test for movie load and load more movies flowfrom mocked data

• `testMoviesLoadFlowFromServer()`

> A test case to test for movie load from server

• `testImageDownloadAndCaching()`

> Test the caching functionality of Image Caching Helpers

## Shortcuts
 - Have used Interface builder for creating layouts. Could have created everything programmatically(in `loadView()`) for better performance. We use to do that in Zomato Restaurant app and it makes a noticeable difference to scroll performance. 

 - The method to initialize `rootViewController` in AppDelegate is very simple. It gets more complicated as the app features evolve. Could have used some common manager to handle all the navigation/routing part.

 - Should have used Reactive pattern to communicate from ViewModel to View.

 - A separate framework or target for all the network layer code can be done. It makes the framework reusable between projects and maintains a line of modularity. We had that structure in Zomato where we had different projects like ZUIKit, ZUINetwork etc. Careem has multiple apps(Food app), so they can have separate frameworks for UI, Networking and Service layers and other core functionality.

 - A better app user experience,like already loading some movies(which are trending from server) when the app begins. Having a different screen to search images which also shows search suggestions(trending searches from server) when `UISearchBar` is active

## Time estimates

The overall app took me about 16-18 hours including refreshment breaks :). Unit testing and writing this **ReadMe**  file also contributed to that time. 
