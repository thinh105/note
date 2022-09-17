---
id: au5rgcf0tffa4e41b406xoi
title: Mobx
desc: ''
updated: 1663409164563
created: 1663409164563
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Mobx
updated_imported: '2022-01-11T03:13:23.000Z'
created_imported: '2022-01-11T03:09:44.000Z'
---

# Tip to Get the Mobx in functional component in legacy project


```js
import { MobXProviderContext } from "mobx-react";
//...
export const PaymentBaseFields = (props: Props) => {
	const { store } = React.useContext(MobXProviderContext);

	const currencySymbol = store.restaurant?.settings.region.currency.symbol || "$";
```

Further reading:
- For legacy Project: https://mobx-react.js.org/recipes-migration
- For New Project: https://mobx-react.js.org/recipes-context
- [Document `MobXProviderContext` issue:](https://github.com/mobxjs/mobx-react/issues/689) 