---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/Quanda/shelf'>View Shelf on GitHub</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Shelf REST API docs! You can use this API to access Shelf API endpoints, which facilitates requests to manage
your digital bookshelf.

# Registration
> To register with Shelf, use this code:

```shell
curl -X POST
  https://shelf-app.herokuapp.com/api/users
  -H 'Content-Type: application/json'
  -d '{
	"fullname": "<your fullname>",
	"username": "<your username>",
	"password": "<your password>"
}	'
```
Shelf uses JSON Web Token's to control access to the API. In order to get a token, you first must be
registered with an account. To do so, either head over the <a href='https://shelf-app.herokuapp.com/signup.html'>Signup page</a> or make a POST request to the `users` endpoint as in the example on the right.
You only need to register once.

### HTTP Request

`POST https://shelf-app.herokuapp.com/api/users`

### Required fields
Field | Description
--------- | -----------
fullname | First and last name
username | Your desired username
password | Your desired password

If successful, you should see a 201 response code.

# Authentication
> To get an API token, use this code:

```shell
curl -X POST
  https://shelf-app.herokuapp.com/api/auth/login
  -H 'Content-Type: application/json'
  -d '{
	"username": "<your username>",
	"password": "<your password>"
}	'
```
Once you are registered, to get a token, make a POST request to this endpoint 
as in the example on the right using your Username and Password. 

### HTTP Request

`POST https://shelf-app.herokuapp.com/api/auth/login`

### Required fields
Field | Description
--------- | -----------
username | Your username
password | Your password

If successful, this endpoint returns a 7 day expiry token. Add that token in the header of your subsequent requests to the API
via Bearer Authentication for your requests to be authenticated. The header should look as follows:

`Authorization: Bearer <yourapitoken>`

<aside class="notice">
You must replace <code>&lt;yourapitoken&gt;</code> with your personal API token.
</aside>


# Books

## Find book
> For example, to search for books related to J.R.R Tolkien, use this code:

```shell
curl -X GET
  https://shelf-app.herokuapp.com/api/findbook/J. R. R. Tolkien
```

> The above command returns a 200 status code and an array of the top 5 books matching your search. 
In this example, the return data would be structured like this:

```json
[
    {
        "id": "zWzAAgAAQBAJ",
        "selfLink": "https://www.googleapis.com/books/v1/volumes/zWzAAgAAQBAJ",
        "volumeInfo": {
            "title": "J.R.R. Tolkien",
            "subtitle": "A Biography",
            "authors": [
                "Humphrey Carpenter"
            ],
            "publisher": "Houghton Mifflin Harcourt",
            "publishedDate": "2014-03-04",
            "description": "The authorized biography of the creator of Middle-earth. In the decades since his death in September 1973, millions have read THE HOBBIT, THE LORD OF THE RINGS, and THE SILMARILLION and become fascinated about the very private man behind the books. Born in South Africa in January 1892, John Ronald Reuel Tolkien was orphaned in childhood and brought up in near-poverty. He served in the first World War, surviving the Battle of the Somme, where he lost many of the closest friends he'd ever had. After the war he returned to the academic life, achieving high repute as a scholar and university teacher, eventually becoming Merton Professor of English at Oxford where he was a close friend of C.S. Lewis and the other writers known as The Inklings. Then suddenly his life changed dramatically. One day while grading essay papers he found himself writing 'In a hole in the ground there lived a hobbit' -- and worldwide renown awaited him. Humphrey Carpenter was given unrestricted access to all Tolkien's papers, and interviewed his friends and family. From these sources he follows the long and painful process of creation that produced THE LORD OF THE RINGS and THE SILMARILLION and offers a wealth of information about the life and work of the twentieth century's most cherished author.",
            "readingModes": {
                "text": true,
                "image": true
            },
            "maturityRating": "NOT_MATURE",
            "allowAnonLogging": true,
            "contentVersion": "0.9.7.0.preview.3",
            "panelizationSummary": {
                "containsEpubBubbles": false,
                "containsImageBubbles": false
            },
            "imageLinks": {
                "smallThumbnail": "http://books.google.com/books/content?id=zWzAAgAAQBAJ&printsec=frontcover&img=1&zoom=5&edge=curl&source=gbs_api",
                "thumbnail": "http://books.google.com/books/content?id=zWzAAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api"
            },
            "previewLink": "http://books.google.com/books?id=zWzAAgAAQBAJ&printsec=frontcover&dq=J.+R.+R.+Tolkien&hl=&cd=1&source=gbs_api",
            "infoLink": "https://play.google.com/store/books/details?id=zWzAAgAAQBAJ&source=gbs_api",
            "canonicalVolumeLink": "https://market.android.com/details?id=book-zWzAAgAAQBAJ"
        }
    },
    {
      ...next book
    }
]
```

This endpoint returns details about the top 5 books matching your search, 
acting as a wrapper for the Google Books API.

This endpoint is not protected and therefore does not require an API token in the request header.


### HTTP Request

`GET https://shelf-app.herokuapp.com/api/findbook/<search_string>`

<aside class="notice">
You must replace <code> &lt;search_string&gt;</code> with your search string.
</aside>


## Add Book
> For example, to add The Fellowship of the Ring, use this code:

```shell
curl -X POST
  https://shelf-app.herokuapp.com/api/users/books
  -H 'Authorization: Bearer <yourapitoken>'
  -H 'Content-Type: application/json'
  -d '{
	"title": "The Lord of the Rings: The Fellowship of the Ring",
	"author": "J. R. R. Tolkien",
	"isbn": "9780547951942"
}'
```

> The above command returns a 200 status code and JSON body structured like this:

```json
{
    "title": "The Lord of the Rings: The Fellowship of the Ring",
    "author": "J. R. R. Tolkien",
    "isbn": "9780547951942"
}
```

This endpoint adds a book to your Shelf.

### HTTP Request

`POST https://shelf-app.herokuapp.com/api/users/books`

### Required fields
Field | Description
--------- | -----------
title | Title of the book
author | Author of the book
isbn | ISBN of the book

### Optional fields
Field | Description
--------- | -----------
description | A description of the book
rating_user | A rating from 0-5
image_link | An image URL of the book cover


## Get all Books
> To retrieve all books from your Shelf, use this code:

```shell
curl -X GET
  https://shelf-app.herokuapp.com/api/users/books
  -H 'Authorization: Bearer <yourapitoken>
```

> The above command returns a 200 status code and an array of book objects structured like this:

```json
{
    "_id": "5bbb66bf20bd240014fa351b",
    "books": [
        {
            "_id": "5bbd57953161900014b9ebef",
            "title": "The Lord of the Rings: The Fellowship of the Ring",
            "author": "J. R. R. Tolkien",
            "isbn": "9780547951942"
        },
        {
          ...next book
        },
        {
          ...next book
        }
    ]
}
```

This endpoint returns all books on your Shelf.


### HTTP Request

`GET https://shelf-app.herokuapp.com/api/users/books`



## Get Book
> For example, to retrieve The Fellowship of the Ring from your Shelf, use this code:

```shell
curl -X GET
  https://shelf-app.herokuapp.com/api/users/books/9780547951942
  -H 'Authorization: Bearer <yourapitoken>
```

> The above command returns a 200 status code and an object structured like this:

```json
{
    "_id": "5bbd57953161900014b9ebef",
    "title": "The Lord of the Rings: The Fellowship of the Ring",
    "author": "J. R. R. Tolkien",
    "isbn": "9780547951942"
}
```

This endpoint returns a single book from your Shelf.


### HTTP Request

`GET https://shelf-app.herokuapp.com/api/users/books/<ISBN>`


## Delete Book
> For example, to delete The Fellowship of the Ring from your Shelf, use this code:

```shell
curl -X DELETE
  https://shelf-app.herokuapp.com/api/users/books/9780547951942
  -H 'Authorization: Bearer <myapitoken>
```

> The above command returns a 204 status code and an empty body


This endpoint deletes a single book from your Shelf.


### HTTP Request

`DELETE https://shelf-app.herokuapp.com/api/users/books/<ISBN>`