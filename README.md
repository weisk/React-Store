# React + Stripe Ecommerce Store Template

Written in React, served up with Express, and integrated with [Stripe Dashboard](https://stripe.com/us/payments).

Uses [Material-UI](http://material-ui.com/) and [styled-components](https://www.styled-components.com/) for the design.

Site also includes a password-protected admin view, Nodemailer integration for sending order updates, and email templates built with Handlebars.

### Quickstart

> Go [here](https://react-test-store.herokuapp.com/) to view the live site
> Visit the [START_HERE](https://github.com/Austinmoore1492/React-Store/blob/master/START_HERE.txt) file to see how to set up the Stripe Dashboard and how to use the assets file.

```
npm install
npm start
npm server
```

### Store Config

Individual stores are created via a config file. There are three example configs in `/src/assets/`. The config generally looks like:

```
{
  "store_name": "Awesome E-Commerce Store",
  "store_slug": "react-stripe-store",
  "api_key": {STRIPE_PUBLIC_KEY}, //Get this from your stripe Dashboard
  "colors": {
    "primary": {
      "main": "#003b6f",
      "dark": "#1d1d1d",
      "contrastText": "#FFF"
    },
    "secondary": {
      "main": "#ff5100"
    }
  },
  "products": [
    {
      "name": "Awesome T-Shirt",
      "url": "url-for-product", //This sets the URL for the shop
      "stripe_id": {STRIPE_PRODUCT_ID},
      "description": "T-Shirt!",
      "photos": ["photo1.jpg","photo2.jpg"], //These should be in the public folder
      "details": [
        "These are details that get rendered as bullet points",
        "Useful for short + sweet info"
      ]
    },
  ...
  ]
}
```

### Product ID and SKU

In your Stripe Dashboard go to Product, underneath the Order Section on the left, to create a product. Use the Product ID it gives you for the above Product ID. You can create a SKU inside of each individual product and set the price for each product from there.

Each product can also have an optional `variants` key for additional metadata to be saved for each product, rendered as a dropdown. This is for saving options without having to create individual SKUs in Stripe.

```
"variants": [
  {
    "name": "metadata",
    "options":[
      {"label": "option 1"},
      {"label": "option 2"}
    ]
  }
]
```

Items in that config in all caps are sourced from Stripe. This project makes use of Stripe Dashboard to keep track of Product inventories, and SKUs (this allows Stripe to handle all payment info, reducing the risk of man-in-the-middle issues). On loading a product page, this site will ask Stripe for the SKUs associated with the given product ID.

Items added to the cart are saved via `localStorage`, which namespaces them according to the `store_slug`, such that you can run several stores at once and keep each purchase separate.

Images are expected to live in `/public/photos/{product.url}/{product.photos.name}`. The site will also add a CSS class on the body that is the store slug, for store-specific CSS.

### Admin View

Orders can be tracked at `/admin`, which is accessed via `/login`. The admin password is saved as an env variable, and admin status is saved in a session cookie.

Secrets are stored via environment variables, which are created via [dotenv](https://www.npmjs.com/package/dotenv). This process expects a file titled `config.env` in your `/root` folder, with the following items:

```
STRIPE_KEY={get this from stripe}
ADMIN_PW={your password to log in}
SESSION_SECRET={generate a random string here, make it hard to guess}
EMAIL_FROM=
EMAIL_CLIENT_ID=
EMAIL_CLIENT_SECRET=
EMAIL_REFRESH_TOKEN=
EMAIL_ACCESS_TOKEN=
EMAIL_EXPIRES=
```

All of the `EMAIL_` items are generated by the following [Gmail Oauth2 Setup](https://stackoverflow.com/a/43202668).

### Email Templates

The repo comes with multiple order templates, which are triggered by updating the order status on the `/admin` page. All of the relevant personalizations (store name, banner color) come from the config file.

### Deployment

Hosting on Heroku is simple, I deployed by Connecting my GitHub Repo to a Heroku Project. You can add
`"heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install && npm run build "` to build to application on the heroku server instead of building it on your computer and uploading the build files.

**Remember to set up the `config.env` file!**
