---
id: 4x00kh6b2lyk35j0954fjv7
title: External Package
desc: ''
updated: 1663409164562
created: 1663409164562
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: External Package
updated_imported: '2022-01-07T16:57:49.000Z'
created_imported: '2022-01-07T16:50:19.000Z'
---

[TOC]


# Formik - FastField vs Field

**FastField has a shouldComponentUpdate which is 'concerned' about changes to only its own props.**

If, your use-case, requires field to re-render because of any other change then don't go for FastField.

Even if some other prop change, your component won't be updated.

Also, as per the docs, performance issues due to Formik Field's re-render might surface only on cases where the form is huge (>30 Fields). They recommend using FastFields for >30 Fields only.

## My case:

![b534bb296f779eb83d08cd27e10d1a24.png](/assets/b534bb296f779eb83d08cd27e10d1a24-tmol0lay3atr.png)

The `Highlighted Detail Fields` takes options from the `Detail Customization`

But if the `Detail Customization` get some update, the `Highlighted Detail Fields` will not get the updated data if we use `FastField` in here.

We should use `Field` for this case.

![e8844fa299e6936570d0418ac44e558a.png](/assets/e8844fa299e6936570d0418ac44e558a-uajxq1ho5zif.png)

