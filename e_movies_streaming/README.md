## eMovies streaming remodel
Aim of this project was to modernize the movie rental erwin model. In an effort to shed as much bloat as possible, I opted in to create a new model from scratch. My solution is to create a movie streaming rental service with the following capabilities:

- User can sign-in for this service using their phone number and one time password automatically sent to their phone
    - If the user doesn't already exist in the database, user will fill-out a form with their email and region to sign-up
- User can rent and stream movies from the available catalog of movies that are also available to their region
- User payment is stored and handled by a third party payment provider
- User can search movies by movie title, genre and actor name
- Movie details such as description, release date, plot summary and more are handled by IMDB API

## Solution
<img width="997" alt="Model screenshot" src="https://github.com/cosmicRover/erwin_projects/assets/41096232/27a8c7f9-7957-4efd-84f9-a008d20708b7">
