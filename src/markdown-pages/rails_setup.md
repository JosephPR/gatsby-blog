---
title: 'Rails Setup'
date: '2019-06-21'
description: 'Setting up one-to-many relationships for a photo album app in Rails'
---

Today we will be building a simple Ruby on Rails application that allows you to store photos in Albums that you create as well. First things first, this article assumes you have basic knowledge of the Ruby language and that you are familiar with rails. I will go through the steps of creating an application in Rails but will not be explaining everything in detail, follow along to get a nifty app to store your photos.

![carey](https://media.licdn.com/dms/image/C5612AQFIb0E4dvR7zA/article-inline_image-shrink_1000_1488-alternative/0?e=1570060800&v=beta&t=oFiOWmQV3rWFnr554SI8KotuwtyAfph0ckDQiSBbeFY)

1. Open your terminal get to the directory that you want to keep the folder for your application and create a new rails app

```ruby
rails new photo_app
```

2. Great! now make sure you cd into your new app and then we can generate our model and controllers, your single source of truth should always be generated first.

```ruby
rails g resource Album
rails g resource Photo
```
3. Now we have plenty to work with. Go into your Models folder that we just created and set up your relationships, An album can have many photos and a photo has one an album.

```ruby
class Album < ApplicationRecord
  has_many :photos
end

class Photo < ApplicationRecord
  has_one :album
end
```

4. Before we can run our server we need to set up our tables, we generated routes and a few empty views folders we will get to soon, but first, let's look at our migrate folder, inside our db folder, and give our albums a name.

```ruby
class CreateAlbums < ActiveRecord::Migration[5.2]
  def change
    create_table :albums do |t|
      t.string :name
      t.timestamps
    end
  end
end
```
5. Next, we set up our photos table with a name and references :album so we get an album id when we create our table.

```ruby
class CreatePhotos < ActiveRecord::Migration[5.2]
  def change
    create_table :photos do |t|
      t.string :name
      t.string :image_url
      t.references :album
      t.timestamps
    end
  end
end
```

6. Save, open your terminal and proceed with the following:

7.rails db:migrate

8. rails server

9. go to localhost:3000 in your browser and Yay we are on Rails!

```ruby
rails db:migrate
rails server
```

Your schema.rb should look like this

```ruby
create_table "albums", force: :cascade do |t|
  t.string "name"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
end


create_table "photos", force: :cascade do |t|
  t.string "name"
  t.string "image_url"
  t.integer "album_id"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
  t.index ["album_id"], name: "index_photos_on_album_id"
end
```

Now we have to set up our views in order to get something other than an error or the generic rails welcome app.

10. In the views/albums folder that was created for us lets add an index.html.erb.

11. Set up a standard HTML file with an h1 tag that says "Welcome to PhotoAlbum".

```ruby
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Albums</title>
  </head>
  <body>


    <h1>Welcome to PhotoAlbums</h1>


  </body>
</html>
```

12. Go into your controller's folder and inside the albums_controller.rb set up an index method.

```ruby
class AlbumsController < ApplicationController
  def index
  end
end
```

navigate over to your first view page [albums](http://localhost:3000/albums)

High five yourself, you now have a working rails application.

![top](https://media.licdn.com/dms/image/C5612AQGtJETxug2lVA/article-inline_image-shrink_1500_2232/0?e=1570060800&v=beta&t=-2oTUCjt6nEcC51neueIXn8GSfoMGjaLJYGZqH5LKZM)

If you have successfully gotten to this point of my tutorial then you are well on your way to creating a function photo album app. In the second part of this tutorial, I will show you how to create methods that will allow you to have a photos page where you can view all of your photos and set up the albums controller to allow for photos to be associated to the album you want to keep them in.
