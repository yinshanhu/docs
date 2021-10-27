
## 布局

### box和flex用法对比

```css
display:-webkit-box
display:flex

-webkit-box-orient:row,column
flex-direction:row,column

-webkit-box-align:start,end,center
align-item:flex-start,flex-end,center,stretch

-webkit-box-pack:start,end,center
justify-content:flex-start,flex-end,center,space-between
```

## BaseCss

```css
html,body,form,fieldset,p,div,ul,h1,h2,h3,h4,h5,h6,figure,article,strong,dl,dd{border:0;outline:0;margin:0;padding:0;-webkit-tap-highlight-color:rgba(0,0,0,0);-webkit-text-size-adjust:none}*{-webkit-touch-callout:none;-webkit-user-select:none;margin:0;padding:0;font-family:Arial}input,textarea{-webkit-touch-callout:inherit;-webkit-user-select:text}body,html{width:100%;height:100%}body{background-color:#edf0f5;overflow-x:hidden}ul{list-style:none}a{text-decoration:none!important;outline:0;display:block}a:hover,a:active{outline:0}em,i{font-style:normal}img{border:0 none;vertical-align:middle;width:100%}:focus{outline:0}
```

## Components

```css
.ellipsis { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.ellipsis_2 { overflow: hidden; text-overflow: ellipsis; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; }
.font { font-family: Helvetica Neue,Helvetica,STHeiTi,sans-serif; }
.flex { display: flex; justify-content: center; align-items: center; align-content: center; flex-wrap: wrap;}
.none { display: none; }

.border_right { position: relative; }
.border_right::after { content: ""; position: absolute; top: 0; right: 0; bottom: 0; width: 1px; background: #E4E4E4; transform: scaleX(0.5); transform-origin: right top; }
.border_top { position: relative; }
.border_top::after { content: ""; position: absolute; top: 0; left: 0; right: 0; height: 1px; background: #E4E4E4; transform: scaleY(0.5); transform-origin: left top; }
.border_right { position: relative; }
.border_right::after { content: ""; position: absolute; top: 0; right: 0; bottom: 0; width: 1px; background: #E4E4E4; transform: scaleX(0.5); transform-origin: right top; }
```