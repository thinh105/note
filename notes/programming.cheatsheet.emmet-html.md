---
id: wrnls1lg71t0g9v53369q07
title: Emmet HTML
desc: ''
updated: 1663409164559
created: 1663409164559
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Emmet HTML
updated_imported: '2020-05-14T02:25:33.000Z'
created_imported: '2020-05-14T01:55:22.000Z'
---

[TOC]

## Cheat Sheet
https://docs.emmet.io/cheat-sheet/

## Class `.`
```html
div.right.center

<div class="right center"></div>
```

## Child `>`

```html
div.navigation>div.is-right>router-link

<div class="navigation">
  <div class="is-right">
    <router-link></router-link>
  </div>
</div>
```

## Sibling `+`

```html
div+p+bq
<div></div>
<p></p>
<blockquote></blockquote>
```

## Custom attributes `[abc = "xyz"]`

```html
p[title="Hello world"]
<p title="Hello world"></p>

td[rowspan=2 colspan=3 title]
<td rowspan="2" colspan="3" title=""></td>

[a='value1' b="value2"]
<div a="value1" b="value2"></div>
```

## Example

```html
div.navigation-buttons>div.is-pulled-right>router-link[to="/products"].button>i.fa.fa-user-circle+span{Shop}^routerlink[to="/cart"].button.is-primary>i.fa.fa-shopping-cart+span{{{cartQuantity}}}

<div class="navigation-buttons">
  <div class="is-pulled-right">
    <router-link to="/products" class="button">
    	<i class="fa fa-user-circle"></i>
    	<span>Shop</span>
    </router-link>
    <routerlink to="/cart" class="button is-primary">
    	<i class="fa fa-shopping-cart"></i>
    	<span>{{cartQuantity}}</span>
    </routerlink>
  </div>
</div>
    
    
```