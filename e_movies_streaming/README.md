## eMovies Streaming Remodel

The aim of this project was to modernize the movie rental model in Erwin. To reduce complexities and streamline the process, I decided to create a new model from scratch. The objective was to design a movie streaming rental service with the following features:

- Users can sign in using their phone number and receive a one-time password sent to their phone.
- Users can rent and stream movies available in the catalog that are also accessible in their region.
- Payments made by users are securely stored and processed by a third-party payment provider.
- Users can search for movies by title, genre, and actor name.
- Movie details, such as description, poster, release date, and plot summary, are fetched on demand from the IMDB API using the attribute `MovieIMDBID`.

## Steps

- First, I planned entities, attributes and their relationships on a textpad (detailed below).
- Then, I implemented them on erwin CDM, LDM views using eMovies as a guide.
- After that, I worked on the bridge tables for PDM.
- Finally, I added formatting, renamed relationships, added readme and uploaded to github.

## Solution
<img width="1058" alt="PDM" src="https://github.com/cosmicRover/erwin_projects/assets/41096232/620ce53a-019c-4787-9849-b0ce31df0381">

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
        - MovieIMDBID: String (used to fetch movie details from IMDB on user devices)
        - MovieSteamURL: String (movie storage bucket link)
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
    - Movie to Actor (via MovieActor): Many-to-many (A movie can feature several actors, and an actor can appear in multiple movies.)
    - Movie to Genre (via MovieGenre): Many-to-many (A movie can belong to several genres, and a genre can include multiple movies.)

Default Values:

Since this model uses only essential entities and attributes to meet the requirements, I can enforce non-null requirement quite easily.

Solution notes:

- If the user's phone number isn't already in the database, they will be prompted to submit their email to sign-up for this service.
- The region name will then be extracted from the provided phone number using Twilio.
- User email prefix (the part before '@') will be used as username on the UI.
- For a user to rent/watch a movie, the movie must be available in their region.
- User is able to update their region by updating their phone number
```
