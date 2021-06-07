# books-reviews

> Books+Reviews uses the New York Times Books API to display the bestsellers lists. The user can choose one of the NYT bestseller lists from the dropdown of more than 20 options. Then the user can choose the current list, or travel back in time to see what was popular in past years up to 5 years ago.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

## Dev Notes
The public API I chose to work with is the New York Times Books API: https://developer.nytimes.com/docs/books-product/1/overview
The two parameters that the user can change are the list (there are more than 20 choices ranging from topics like Hardcover Fiction to Young Adult), and the date. I chose to make the date a "time-machine" style, where the user can choose the current date or travel back in time 1 - 5 years.
Technology used: VueJS, ElementUI (a styling library), and axios
With more time, I would do a lot more to expand this:
1. Security with the api-key
2. Splitting the main page into components. I want to make both dropdowns use one flexible "dumb" component, and then I would have a separate component for the actual list. With time to have a details panel (described in #6) I would also make that a separate component. 
3. I don't like the library styling of the dropdowns. I would change that.
4. I would also make sure that when the user chooses a parameter, it would show in the dropdown field. The current is poor User Experience.
5. I want a loading indicator for the books list.
6. I like the simple styling of the page because it looks like how a newspaper would present the list. But there is a lot more that I could do with the list.
  a. I'd make the title clickable, so the user can open details about each title. The API returns title, author, summary, a picture, links to purchase, and links to book reviews.
