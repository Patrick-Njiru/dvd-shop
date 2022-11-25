# Generating a Rails API From Scratch

Sometimes we may want to generate a rails application to only act as an API for a separate frontend application. Let us create an API called "dvd-shop". To start, we navigate to a directory of our choosing and run:
```console
$ rails new dvd-shop --api --minimal
```
- `--api` serves to create our application with configurations specific to APIs.
- `--minimal` ensures only the necessary Rails features are installed and eliminates any features that are not needed for APIs.

## Using the Resource Generator
We now have to create a model(Movie), a controller and a migration , on top of adding the necessary route(s). We wanr our database table for our Movie model to have the following attributes:
| Column Name     | Data Type |
| --------------- | --------- |
| title           | string    |
| year            | integer   |
| length          | integer   |
| director        | string    |
| description     | string    |
| poster_url      | string    |
| category        | string    |
| discount        | boolean   |
| female_director | boolean   |

The model, controller, migration, and route can be created all at once with one command simplifying our work. To do so, navigate into the `dvd-shop` directory and run:
```console
$ rails g resource Movie title year:integer length:integer director description poster_url category discount:boolean female_director:boolean --no-test-framework
```
This will create a `movie.rb` model file inside the `models` folder, a `movies_controller.rb` controller file inside the `controllers` folder, a migration for the `movies` table with the given attributes inside the `migrate` folder, and a `resources :movies` route in the `routes.rb` file inside the `config` folder.
This command should only be run if you need to create all these files at once, otherwise there are other commands that are specific to each individual file.

## Running The API
some data is provided in the `seeds.rb` file that can be used to populate the `movies` table. But we need to run the migration first in order to create the table before adding data to it. To do this at once, run: 
```console
$ rails db:migrate db:seed
```
For now, let us modify our `routes.rb` file so that only one route works for the front end:
```rb
 # config/routes.rb
 resources :movies, only: [:index]
 ```
 For this to work, we need to and an index method to our controller as follows:
 ```rb
 # controllers/movies_controller.rb
 def index
  movies =Movie.all
  render json: movies
 end
 ```
 Now that everything is setup, you can run `rails s` to start the server and navigate to `http://localhost:3000/movies` in your browser to view the movies data in the database.
