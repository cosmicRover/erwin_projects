## eMovies Streaming Remodel

The aim of this project was to modernize the movie rental model in Erwin. To reduce complexities and streamline the process, I decided to create a new model from scratch. The objective was to design a movie streaming rental service with the following features:

- Users can sign in using their phone number and receive a one-time password sent to their phone.
    - If the user's phone number isn't already in the database, they will be prompted to fill out a form with their email. The region name will then be extracted from the provided phone number using Twilio.
    - User email prefix (the part before '@') will be used as username on the UI
- Users can rent and stream movies available in the catalog that are also accessible in their region.
- Payments made by users are securely stored and processed by a third-party payment provider.
- Users can search for movies by title, genre, and actor name.
- Movie details such as description, release date, and plot summary are sourced from the IMDB API.

Solution

## Solution
<img width="997" alt="Model screenshot" src="https://github.com/cosmicRover/erwin_projects/assets/41096232/27a8c7f9-7957-4efd-84f9-a008d20708b7">

Below are the entities and their relationships for this model

```
Entities and Attributes:

    User:
        - UserID: Int (Primary Key, Auto-incremented)
        - Email: String
        - PhoneNumber: String (Unique)
        - Region: String

    Movie:
        - MovieID: Int (Primary Key, Auto-incremented)
        - Title: String
        - MovieIMDBID: String
        - MovieSteamURL: String
        - AvailableRegions: String (Comma-separated values e.g., "region1, region2, region3...")

    Genre:
        - GenreID: Int (Primary Key, Auto-incremented)
        - Name: String

    Actor:
        - ActorID: Int (Primary Key, Auto-incremented)
        - Name: String

    MovieActor (Bridge table between Movie and Actor):
        - MovieActorID: Int (Primary Key, Auto-incremented)
        - MovieID: Foreign Key (referring to Movie)
        - ActorID: Foreign Key (referring to Actor)

    MovieGenre (Bridge table between Movie and Genre):
        - MovieGenreID: Int (Primary Key, Auto-incremented)
        - MovieID: Foreign Key (referring to Movie)
        - GenreID: Foreign Key (referring to Genre)

    Rental:
        - RentalID: Int (Primary Key, Auto-incremented)
        - UserID: Foreign Key (referring to User)
        - MovieID: Foreign Key (referring to Movie)
        - RentalDate: Date
        - ExpiryDate: Date

    Payment (Inserted by a third-party payment provider):
        - PaymentID: Int (Primary Key, Auto-incremented)
        - UserID: Foreign Key (referring to User)
        - Amount: Decimal
        - PaymentDate: Date
        - EPaymentId: String

Entity Relationships:

    - User to Rental: One-to-many (A single user can have multiple rentals, but each rental is specific to one user.)
    - Movie to Rental: One-to-many (One movie can be rented by various users, but each rental refers to a single movie.)
    - User to Payment: One-to-many (A user can make several payments, but each payment record corresponds to one user.)
    - Movie to Genre: Many-to-one (Every movie is linked to a genre, but a genre can encompass numerous movies.)
    - Movie to Actor (via MovieActor): Many-to-many (A movie can feature several actors, and an actor can appear in multiple movies.)
    - Movie to Genre (via MovieGenre): Many-to-many (A movie can belong to several genres, and a genre can include multiple movies.)

Default Values:

Since this model uses only essential entities and attributes to meet the requirements, I can enforce non-null requirement quite easily.
```