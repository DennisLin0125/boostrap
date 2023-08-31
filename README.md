# 專案介紹 : bootstrap

## 基本參數

```less
//** Number of columns in the grid.
@grid-columns:              12;
//** Padding between columns. Gets divided in half for the left and right.
@grid-gutter-width:         30px;
// Navbar collapse
//** Point at which the navbar becomes uncollapsed.
@grid-float-breakpoint:     @screen-sm-min;
//** Point at which the navbar begins collapsing.
@grid-float-breakpoint-max: (@grid-float-breakpoint - 1);
```

## Media queries breakpoints

* Define the breakpoints at which your layout will change, adapting to different screen sizes.

```less
// Extra small screen / phone
//** Deprecated `@screen-xs` as of v3.0.1
@screen-xs:                  480px;
//** Deprecated `@screen-xs-min` as of v3.2.0
@screen-xs-min:              @screen-xs;
//** Deprecated `@screen-phone` as of v3.0.1
@screen-phone:               @screen-xs-min;

// Small screen / tablet
//** Deprecated `@screen-sm` as of v3.0.1
@screen-sm:                  768px;
@screen-sm-min:              @screen-sm;
//** Deprecated `@screen-tablet` as of v3.0.1
@screen-tablet:              @screen-sm-min;

// Medium screen / desktop
//** Deprecated `@screen-md` as of v3.0.1
@screen-md:                  992px;
@screen-md-min:              @screen-md;
//** Deprecated `@screen-desktop` as of v3.0.1
@screen-desktop:             @screen-md-min;

// Large screen / wide desktop
//** Deprecated `@screen-lg` as of v3.0.1
@screen-lg:                  1200px;
@screen-lg-min:              @screen-lg;
//** Deprecated `@screen-lg-desktop` as of v3.0.1
@screen-lg-desktop:          @screen-lg-min;
```

## Container sizes

* Define the maximum width of `.container` for different screen sizes.

```less
// Small screen / tablet
@container-tablet:             (720px + @grid-gutter-width);
//** For `@screen-sm-min` and up.
@container-sm:                 @container-tablet;

// Medium screen / desktop
@container-desktop:            (940px + @grid-gutter-width);
//** For `@screen-md-min` and up.
@container-md:                 @container-desktop;

// Large screen / wide desktop
@container-large-desktop:      (1140px + @grid-gutter-width);
//** For `@screen-lg-min` and up.
@container-lg:                 @container-large-desktop;
```

## Container widths

* Set the container width, and override it for fixed navbars in media queries.

```less
.container {
  .container-fixed();

  @media (min-width: @screen-sm-min) {
    width: @container-sm;
  }
  @media (min-width: @screen-md-min) {
    width: @container-md;
  }
  @media (min-width: @screen-lg-min) {
    width: @container-lg;
  }
}
```

## Class 行

* 可以調整為幾等份
* 常用class:
  * `col-xs-1` .... `col-xs-12`
  * `col-sm-1` .... `col-sm-12`
  * `col-md-1` .... `col-md-12`
  * `col-lg-1` .... `col-lg-12`

### CSS屬性

```css
position: relative;  
min-height: 1px;    // Prevent columns from collapsing when empty
padding-right: 15;  // Inner gutter via padding
padding-left: 15;
```

### 源代碼

```less
.make-grid-columns() {
  // Common styles for all sizes of grid columns, widths 1-12
  .col(@index) { // initial
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general; "=<" isn't a typo
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      position: relative;
      // Prevent columns from collapsing when empty
      min-height: 1px;
      // Inner gutter via padding
      padding-right: floor((@grid-gutter-width / 2));
      padding-left: ceil((@grid-gutter-width / 2));
    }
  }
  .col(1); // kickstart it
}

.float-grid-columns(@class) {
  .col(@index) { // initial
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general
    @item: ~".col-@{class}-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      float: left;
    }
  }
  .col(1); // kickstart it
}
```

## Class Push

* 將列向右側推動，這意味著在螢幕畫面下，列會在其所在的行中向右移動
* 常用class:
  * `col-xs-push-1`....`col-xs-push-12`
  * `col-sm-push-1`....`col-sm-push-12`
  * `col-md-push-1`....`col-md-push-12`
  * `col-lg-push-1`....`col-lg-push-12`

```css
left: (1/12)~(12/12);
```

* class移動0格時
  * `col-xs-push-0`
  * `col-sm-push-0`
  * `col-md-push-0`
  * `col-lg-push-0`

```css
left: auto;
```

## 源代碼

```less
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index > 0) {
  .col-@{class}-push-@{index} {
    left: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = push) and (@index = 0) {
  .col-@{class}-push-0 {
    left: auto;
  }
}
```

## Class Pull

* 將列從左側拉回，這意味著在螢幕畫面下，列會在其所在的行中向左移動。
* 常用class:
  * `col-xs-pull-1`....`col-xs-pull-12`
  * `col-sm-pull-1`....`col-sm-pull-12`
  * `col-md-pull-1`....`col-md-pull-12`
  * `col-lg-pull-1`....`col-lg-pull-12`

```css
right: (1/12)~(12/12);
```

* class移動0格時
  * `col-xs-pull-0`
  * `col-sm-pull-0`
  * `col-md-pull-0`
  * `col-lg-pull-0`

```css
left: auto;
```

## 源代碼

```less
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index > 0) {
  .col-@{class}-pull-@{index} {
    right: percentage((@index / @grid-columns));
  }
}
.calc-grid-column(@index, @class, @type) when (@type = pull) and (@index = 0) {
  .col-@{class}-pull-0 {
    right: auto;
  }
}
```

## Class offset

* 可用於添加左側的間距
* 常用class:
  * `col-xs-offset-0`....`col-xs-offset-12`
  * `col-sm-offset-0`....`col-sm-offset-12`
  * `col-md-offset-0`....`col-md-offset-12`
  * `col-lg-offset-0`....`col-lg-offset-12`

```css
margin-left: (1/12)~(12/12);
```

## 源代碼

```less
.calc-grid-column(@index, @class, @type) when (@type = offset) {
  .col-@{class}-offset-@{index} {
    margin-left: percentage((@index / @grid-columns));
  }
}
```