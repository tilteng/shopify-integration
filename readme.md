# Shopify integration with Tilt.com

## Enable merchants to use Tilt campaigns on their Shopify store

### Demo:

Check out our store at https://gettilted.myshopify.com/collections/all
Password: quaert

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/demo.png)


### Installation
#### Modify your Shopify template
Inside your Shopify store admin, go to your theme manager (more details [here](https://help.shopify.com/themes/customization#view-the-customize-theme-page))

Then select `customize theme`

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/customize_your_theme.png)

Inside the `Theme Options`, select `Edit HTML/CSS`

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/select_html_css.png)


##### Modify the theme.liquid file
On the left sidebar, choose your `theme.liquid` file under `Layout`

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/theme_liquid.png)

Before the `</body>` tag, add the following script:

`<script type="text/javascript" src="https://www.tilt.com/new/built/merchant-tools.0.1.js"></script>`

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/add_script.png)

##### Modify the product.liquid file
On the left sidebar, choose your `product-grid-item.liquid` file under `Snippets`

###### Simple
If you are using the default Shopify theme, or a non-modified version of this file, you can replace its content completely by [this](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/liquid/snippets/product-grid-item.liquid)

You can then skip the Advanced part of this manual.

###### Advanced
####### Step 1:

Before the `<div class="grid__item">` tag, add the following lines:

```
{% assign tiltMetadatas = product.metafields.tilt %}
{% assign key = 'tiltId' %}
```

Then replace the tag:

`<div class="grid__item">`

by

`<div class="grid__item" data-campaign="{{ tiltMetadatas[key] }}">`

####### Step 2:
Replace the tag:

`<a href="{{ product.url | within: collection }}" class="product__image" title="{{ product.title | escape }}">`

by

`<a target="_blank" href="https://www.tilt.com/tilts/{{ tiltMetadatas[key] }}" class="grid-link text-center">`

####### Step 3:
Add special Tilt tags (the complete list can be found [here](#list-of-special-tilt-tags))

Example:
```
<div class="tilt_bar">
  <div data-tilt-property="tilt_percent"></div>
</div>
```

or
```
<p data-tilt-property="quantity_text"></p>
```

##### Add a stylesheet
On the left sidebar, choose your `theme.liquid` file under `Layout`

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/theme_liquid.png)

After the `<!-- CSS ===` comment, add the following line:

`{{ 'tilt.scss.css' | asset_url | stylesheet_tag }}`

On the left sidebar, under the Assets section, add a file named `tilt.scss.liquid`.

You can add in here the CSS related to the Tilt widget.

You can also copy paste the content of [this file](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/liquid/assets/tilt.scss.liquid).

### Add a metafield to your products
We will use a metafield on each of your product to link your Tilt campaign with your own Shopify product.

To add a metafield you have multiple solutions, have a look at the documentation [here](https://help.shopify.com/themes/liquid/objects/metafield).

We chose to use `MetaFields Editor`.

Once installed, go to Apps > Installed > Metafields Editor

For each of your product that you want to link with Tilt, enter:

```
Namespace: tilt
Key: tiltId
Value: your campaign slug
```

Example:

![alt tag](https://raw.githubusercontent.com/tilteng/shopify-integration/master/docs/images/metadata.png)

### List of special Tilt tags:
| data-tilt-property value  | Description | Example |
| ------------- | ------------- | ------------- |
| tilt_amount | Amount minimum for the collect campaign to tilt | 100 |
| currency_code | Campaign currency code | USD |
| min_quantity | Minimum # sales required (Sell Something Campaign)| 10 |
| title | Tilt campaign title | Tilt Ping Pong Balls |
| current_quantity | Number of items sold | 28 |
| admin.firstname | First name of the merchant | Lauren |
| admin.img | Image of the merchant | [http://...](https://d2w04addmnh2aq.cloudfront.net/api/file/zNHezFvxQEiaD1QlqMwO) |
| admin.lastname | Last name of the merchant | James |
| tilt_percent | Progress percentage | 187 |
| contributions | Number of contributions | 178 |
| expires_in.hours | Number of hours before expiration | 2 |
| expires_in.seconds | Number of seconds before expiration | 31 |
| expires_in.minutes | Number of minutes before expiration | 21 |
| expires_in.days | Number of days before expiration | 2 |
| img | Image of the Tilt campaign | [http://...](https://d2w04addmnh2aq.cloudfront.net/api/file/S8zq2XLMQKqRoI9O6Tuk) |
| imgSrc | Image added in the element `src` attribute | src="[http://...](https://d2w04addmnh2aq.cloudfront.net/api/file/S8zq2XLMQKqRoI9O6Tuk)" |
| imgBg | Image added in the element `style` attribute | style="background:url([http://...](https://d2w04addmnh2aq.cloudfront.net/api/file/S8zq2XLMQKqRoI9O6Tuk))" |
| contributors | Number of contributors to the Tilt | 172 |
| max_quantity | Max items available | 400 |
| contributors | Number of contributors to the Tilt | 172 | |
| max_quantity | Max items available | 400 |
| raised_amount | Current amount raised | 400 |
| incamount_text | Item price with currency | $2 |
| contribute_text | Contribution text translated in the user language | `Buy it now` (SS campaign) or `Contribute` (Collect campaign) |
| quantity_text | Campaign status translated in the user language | 4 sold (SS campaign) or 100$ collected (Collect campaign) |