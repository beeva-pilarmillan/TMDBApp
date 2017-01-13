## TMDB API

Se utiliza la API v3 de [The Movie DB](https://www.themoviedb.org/) para proveer a la aplicación con información de películas y series de televisión. Actualmente se han implementado las siguientes operaciones:

###Películas

* `search`: Búsqueda en base a una entrada de texto.
* `discover`: Búsqueda basada en filtros como puntuación media, número de votos, género, región... Por defecto no se aplican filtros.
* `popular`: Lista de las películas más populares actualmente en The Movie DB. Se actualiza diariamente.
* `detail`: Detalle de una película, por ejemplo el identificador IMDB.
* `rate`: Votar una película con una puntuación entre 0.5 y 10.

Estas operaciones se pueden utilizar desde la clase `MoviesDataSource` y las respuestas se reciben implementando el protocolo `MoviesDataSourceOutput`.

```swift
public protocol MoviesDataSource {
    weak var moviesDatasourceOutput: MoviesDataSourceOutput? { get set }
    func getMovieDetail(movieId: Int)
    func discoverMovies(page: Int?)
    func getPopularMovies()
    func rateMovie(movieId: Int, score: Double)
    func searchMovie(withText text: String, page: Int?)
    func getImage(withPath path: String, completionHandler: @escaping (_ data: Data?, _ error: Error?) -> Void)
    init(moviesDatasourceOutput: MoviesDataSourceOutput?)
}

public protocol MoviesDataSourceOutput: class {
    func onMovieDetailSuccess(movie: MovieItem?)
    func onMovieDetailError(error: Error?)
    
    func onDiscoverMoviesSuccess(movies: MovieDiscoverModel?, currentPage: Int, totalPages: Int, totalMovies: Int)
    func onDiscoverMovieError(error: Error?)
    
    func onPopularMoviesSuccess(popularMovies: MoviePopularModel?)
    func onPopularMoviesError(error: Error?)
    
    func onSearchMovieSuccess(movies: [MovieItem]?, currentPage: Int, totalPages: Int, totalMovies: Int)
    func onSearchMovieError(error: Error?)
    
    func onRateMovieSuccess()
    func onRateMovieError(error: Error?)
}
```
Para crear una instancia de `MoviesDataSource`, haremos lo siguiente:

```swift
var moviesDataSource = MoviesDataSourceImpl(moviesDatasourceOutput: self)
```

####Ejemplo de uso

```swift
import UIKit
import TMDBApi

class ViewController: UIViewController {

    var moviesDataSource: MoviesDataSource!
   
    override func viewDidLoad() {
        super.viewDidLoad()
        
        moviesDataSource = MoviesDataSourceImpl(moviesDatasourceOutput: self)
        moviesDataSource.searchMovie(withText: "star", page: nil)
    }
}

extension ViewController: MoviesDataSourceOutput {
    
    public func onMovieDetailSuccess(movie: MovieItem?) {
        
    }
    
    public func onMovieDetailError(error: Error?) {
        
    }
    
    public func onDiscoverMoviesSuccess(movies: MovieDiscoverModel?, currentPage: Int, totalPages: Int, totalMovies: Int) {
        
    }
    
    public func onDiscoverMovieError(error: Error?) {
        
    }
    
    public func onPopularMoviesSuccess(popularMovies: MoviePopularModel?) {
        
    }
    
    public func onPopularMoviesError(error: Error?) {
        
    }
    
    public func onSearchMovieSuccess(movies: [MovieItem]?, currentPage: Int, totalPages: Int, totalMovies: Int) {
        print("Movies: \(movies)\n")
    }
    
    public func onSearchMovieError(error: Error?) {
        print("Movies error: \(error)\n")
    }
    
    public func onRateMovieSuccess() {
        
    }
    
    public func onRateMovieError(error: Error?) {
    }
}
```

####Series de TV

* `search`: Búsqueda en base a una entrada de texto.
* `discover`: Búsqueda basada en filtros como puntuación media, número de votos, género, región... Por defecto no se aplican filtros.
* `popular`: Lista de las series más populares actualmente en The Movie DB. Se actualiza diariamente.
* `detail`: Detalle de una serie, así como información básica de las temporadas.
* `season-detail`: Detalle de una temporada, como su título, poster, y los episodios que contiene.
* `rate`: Votar una serie de TV con una puntuación entre 0.5 y 10.

Estas operaciones se pueden utilizar desde la clase `TVShowsDataSource` y las respuestas se reciben implementando el protocolo `TVShowsDataSourceOutput`.

```swift
public protocol TVShowsDataSource {
    weak var tvShowsDataSourceOutput: TVShowsDataSourceOutput? { get set }
    func getTVShowDetail(tvShowId: Int)
    func discoverTVShows(page: Int?)
    func getPopularTVShows()
    func rateTVShow(tvShowId: Int, score: Double)
    func searchTVShow(withText text: String, page: Int?)
    func getTVShowSeasonDetail(forTVShowId tvShowId: Int, seasonNumber: Int)
    func getImage(withPath path: String, completionHandler: @escaping (_ data: Data?, _ error: Error?) -> Void)
    init(tvShowsDataSourceOutput: TVShowsDataSourceOutput?)
}

public protocol TVShowsDataSourceOutput: class {
    func onTVShowDetailSuccess(tvShow: TvShowItem?)
    func onTVShowDetailError(error: Error?)
    
    func onDiscoverTVShowSuccess(tvShows: TVShowDiscoverModel?, currentPage: Int, totalPages: Int, totalTVShows: Int)
    func onDiscoverTVShowError(error: Error?)
    
    func onPopularTVShowSuccess(popularTVShows: TVShowDiscoverModel?)
    func onPopularTVShowsError(error: Error?)
    
    func onSearchTVShowSuccess(tvShows: [TvShowItem]?, currentPage: Int, totalPages: Int, totalTVShows: Int)
    func onSearchTVShowError(error: Error?)
    
    func onRateTVShowSuccess()
    func onRateTVShowError(error: Error?)
    
    func onTVShowSeasonDetailSuccess(seasonNumber: Int, season: TVShowSeason?)
    func onTVShowSeasonDetailError(error: Error?)
}
```
Para crear una instancia de `TVShowDataSource`, haremos lo siguiente:

```swift
var tvShowsDataSource = TVShowsDataSourceImpl(tvShowsDataSourceOutput: self)
```

####Ejemplo de uso

```swift
import UIKit
import TMDBApi

class ViewController: UIViewController {

    var tvShowsDataSource: TVShowsDataSource!
   
    override func viewDidLoad() {
        super.viewDidLoad()
        
        tvShowsDataSource = TVShowsDataSourceImpl(tvShowsDataSourceOutput: self)
        tvShowsDataSource.searchTVShow(withText: "son", page: nil)
    }
}

extension ViewController: TVShowsDataSourceOutput {
    
    public func onTVShowDetailSuccess(tvShow: TMDBApi.TvShowItem?) {
        
    }
    
    public func onTVShowDetailError(error: Error?) {
        
    }
    
    public func onDiscoverTVShowSuccess(tvShows: TVShowDiscoverModel?, currentPage: Int, totalPages: Int, totalTVShows: Int) {
        
    }
    
    public func onDiscoverTVShowError(error: Error?) {
    
    }
    
    public func onPopularTVShowSuccess(popularTVShows: TVShowDiscoverModel?) {
        
    }
    
    public func onPopularTVShowsError(error: Error?) {
        
    }
    
    public func onSearchTVShowSuccess(tvShows: [TMDBApi.TvShowItem]?, currentPage: Int, totalPages: Int, totalTVShows: Int) {
        print("TV Shows: \(tvShows)\n")
    }
    
    public func onSearchTVShowError(error: Error?) {
        print("TV Shows error: \(error)\n")
    }
    
    public func onRateTVShowSuccess() {
        
    }
    
    public func onRateTVShowError(error: Error?) {
        
    }
    
    public func onTVShowSeasonDetailSuccess(seasonNumber: Int, season: TMDBApi.TVShowSeason?) {
        
    }
    
    public func onTVShowSeasonDetailError(error: Error?) {
        
    }
}
```

###Extra

* `multi-search`: Búsqueda de películas y series de televisión en base a una entrada de texto.

Esta operación se pueden utilizar desde la clase `MediaSearchDataSource` y las respuestas se reciben implementando el protocolo `MediaSearchDataSourceOutput`.

Para crear una instancia de `MediaSearchDataSource `, haremos lo siguiente:

```swift
var mediaSearchDatasource = MediaSearchDataSourceImpl(mediaSearchDataSourceOutput: self)
```

####Ejemplo de uso

```swift
import UIKit
import TMDBApi

class ViewController: UIViewController {

    var mediaSearchDatasource: MediaSearchDataSource!
   
    override func viewDidLoad() {
        super.viewDidLoad()
        
        mediaSearchDatasource = MediaSearchDataSourceImpl(mediaSearchDataSourceOutput: self)
        mediaSearchDatasource.searchMedia(forText: "sherlock", page: nil)
    }
}

extension ViewController: MediaSearchDataSourceOutput {
    
    public func onSearchMediaSuccess(movies: [TMDBApi.MovieItem]?, tvShows: [TMDBApi.TvShowItem]?, currentPage: Int, totalPages: Int, totalResults: Int) {
        print("TV Shows: \(tvShows)\n")
        print("Movies: \(movies)\n")
    }
    
    public func onSearchMediaError(error: Error?) {
        print("Media search error: \(error)\n")
    }
}
```

Más información en [https://developers.themoviedb.org/3](https://developers.themoviedb.org/3).

