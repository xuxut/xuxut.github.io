---
title: antd design pro中使用typescript
date: 2019-02-26 11:20:03
tags: 
- react
categories:
- 前端框架 
---

<!-- more -->
## index.js
```javascript
import React, { Component, PureComponent } from 'react'
export default class TagSelect extends Component{

}
```
## TagSelectionOption.d.ts
```javascript
import * as React from 'react';

export interface ITagSelectOptionProps {
  value: string | number;
  style?: React.CSSProperties;
}

export default class TagSelectOption extends React.Component<ITagSelectOptionProps, any> {}
```
## index.d.ts
```javascript
import * as React from 'react'
import TagSelectOption from './TagSelectOption'

export interface ITagSelectProps {
    onChange?: (value: string[]) => void;
    expandable?: boolean;
    value?: string[] | number[];
    style?: React.CSSProperties;
    hideCheckAll?: boolean;
}
export default class TagSelect extends React.Component<ITagSelectProps, any>{
    public static Option: typeof TagSelectOption
    private children:
        | React.ReactElement<TagSelection>
        | Array<React.ReactElement<TagSeletion>>;
}

```