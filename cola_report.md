cola Report for GDS3842
==================

**Date**: 2019-12-25 21:00:53 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 42764    51
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:kmeans](#SD-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[SD:NMF](#SD-NMF)           |          2| 1.000|           1.000|       1.000|** |           |
|[CV:hclust](#CV-hclust)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:kmeans](#CV-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:pam](#CV-pam)           |          6| 1.000|           0.997|       0.998|** |2          |
|[MAD:kmeans](#MAD-kmeans)   |          2| 1.000|           0.960|       0.963|** |           |
|[MAD:skmeans](#MAD-skmeans) |          4| 1.000|           0.993|       0.994|** |2,3        |
|[MAD:pam](#MAD-pam)         |          5| 1.000|           0.932|       0.971|** |2,3,4      |
|[MAD:NMF](#MAD-NMF)         |          2| 1.000|           0.934|       0.976|** |           |
|[ATC:hclust](#ATC-hclust)   |          6| 1.000|           1.000|       1.000|** |2,3        |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:pam](#ATC-pam)         |          3| 1.000|           0.990|       0.994|** |2          |
|[ATC:NMF](#ATC-NMF)         |          2| 1.000|           1.000|       1.000|** |           |
|[SD:pam](#SD-pam)           |          6| 0.972|           0.967|       0.987|** |2,5        |
|[SD:mclust](#SD-mclust)     |          6| 0.969|           0.910|       0.956|** |2          |
|[ATC:mclust](#ATC-mclust)   |          3| 0.969|           0.952|       0.977|** |2          |
|[CV:skmeans](#CV-skmeans)   |          4| 0.968|           0.913|       0.961|** |2          |
|[SD:skmeans](#SD-skmeans)   |          4| 0.960|           0.876|       0.946|** |2          |
|[MAD:hclust](#MAD-hclust)   |          5| 0.960|           0.976|       0.987|** |2          |
|[CV:mclust](#CV-mclust)     |          6| 0.921|           0.949|       0.968|*  |2          |
|[MAD:mclust](#MAD-mclust)   |          4| 0.914|           0.950|       0.971|*  |2,3        |
|[SD:hclust](#SD-hclust)     |          4| 0.912|           0.975|       0.986|*  |2          |
|[ATC:skmeans](#ATC-skmeans) |          6| 0.908|           0.945|       0.941|*  |2,3,4      |
|[CV:NMF](#CV-NMF)           |          5| 0.906|           0.903|       0.939|*  |2          |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:NMF      2     1           1.000       1.000          0.368 0.633   0.633
#&gt; MAD:NMF     2     1           0.934       0.976          0.393 0.613   0.613
#&gt; ATC:NMF     2     1           1.000       1.000          0.368 0.633   0.633
#&gt; SD:skmeans  2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:skmeans  2     1           0.991       0.995          0.372 0.633   0.633
#&gt; MAD:skmeans 2     1           0.988       0.994          0.428 0.576   0.576
#&gt; ATC:skmeans 2     1           1.000       1.000          0.368 0.633   0.633
#&gt; SD:mclust   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:mclust   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; MAD:mclust  2     1           0.995       0.997          0.370 0.633   0.633
#&gt; ATC:mclust  2     1           1.000       1.000          0.368 0.633   0.633
#&gt; SD:kmeans   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:kmeans   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; MAD:kmeans  2     1           0.960       0.963          0.382 0.633   0.633
#&gt; ATC:kmeans  2     1           1.000       1.000          0.368 0.633   0.633
#&gt; SD:pam      2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:pam      2     1           1.000       1.000          0.368 0.633   0.633
#&gt; MAD:pam     2     1           1.000       1.000          0.368 0.633   0.633
#&gt; ATC:pam     2     1           1.000       1.000          0.368 0.633   0.633
#&gt; SD:hclust   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; CV:hclust   2     1           1.000       1.000          0.368 0.633   0.633
#&gt; MAD:hclust  2     1           1.000       1.000          0.368 0.633   0.633
#&gt; ATC:hclust  2     1           1.000       1.000          0.368 0.633   0.633
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.597           0.656       0.768         0.5062 0.746   0.599
#&gt; CV:NMF      3 0.745           0.932       0.911         0.4411 0.788   0.665
#&gt; MAD:NMF     3 0.850           0.915       0.962         0.6925 0.693   0.511
#&gt; ATC:NMF     3 0.638           0.762       0.742         0.4266 0.725   0.566
#&gt; SD:skmeans  3 0.788           0.908       0.934         0.7176 0.704   0.532
#&gt; CV:skmeans  3 0.788           0.963       0.971         0.7479 0.704   0.532
#&gt; MAD:skmeans 3 1.000           0.994       0.997         0.5819 0.746   0.559
#&gt; ATC:skmeans 3 1.000           1.000       1.000         0.2301 0.915   0.866
#&gt; SD:mclust   3 0.832           0.879       0.942         0.4017 0.834   0.742
#&gt; CV:mclust   3 0.818           0.916       0.951         0.3400 0.887   0.824
#&gt; MAD:mclust  3 1.000           0.999       1.000         0.7936 0.704   0.532
#&gt; ATC:mclust  3 0.969           0.952       0.977         0.4697 0.845   0.755
#&gt; SD:kmeans   3 0.601           0.819       0.845         0.6038 0.704   0.532
#&gt; CV:kmeans   3 0.641           0.913       0.857         0.5624 0.704   0.532
#&gt; MAD:kmeans  3 0.645           0.920       0.898         0.6094 0.704   0.532
#&gt; ATC:kmeans  3 0.619           0.739       0.869         0.4885 0.845   0.755
#&gt; SD:pam      3 1.000           0.998       0.998         0.0598 0.979   0.967
#&gt; CV:pam      3 1.000           1.000       1.000         0.0575 0.979   0.967
#&gt; MAD:pam     3 0.914           0.940       0.974         0.7665 0.725   0.566
#&gt; ATC:pam     3 1.000           0.990       0.994         0.2403 0.915   0.866
#&gt; SD:hclust   3 1.000           1.000       1.000         0.0575 0.979   0.967
#&gt; CV:hclust   3 1.000           1.000       1.000         0.0575 0.979   0.967
#&gt; MAD:hclust  3 0.713           0.817       0.813         0.6004 0.718   0.554
#&gt; ATC:hclust  3 1.000           0.978       0.990         0.2638 0.915   0.866
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.737           0.839       0.914         0.2006 0.824   0.595
#&gt; CV:NMF      4 0.793           0.784       0.894         0.2290 0.894   0.761
#&gt; MAD:NMF     4 0.790           0.739       0.868         0.0758 0.947   0.846
#&gt; ATC:NMF     4 0.564           0.712       0.797         0.1426 0.788   0.537
#&gt; SD:skmeans  4 0.960           0.876       0.946         0.1416 0.873   0.652
#&gt; CV:skmeans  4 0.968           0.913       0.961         0.0962 0.965   0.895
#&gt; MAD:skmeans 4 1.000           0.993       0.994         0.0841 0.929   0.786
#&gt; ATC:skmeans 4 1.000           0.993       0.995         0.5407 0.753   0.549
#&gt; SD:mclust   4 0.682           0.867       0.916         0.3283 0.756   0.522
#&gt; CV:mclust   4 0.785           0.926       0.951         0.4260 0.753   0.537
#&gt; MAD:mclust  4 0.914           0.950       0.971         0.0907 0.944   0.832
#&gt; ATC:mclust  4 0.616           0.671       0.837         0.1716 0.894   0.785
#&gt; SD:kmeans   4 0.596           0.791       0.836         0.1568 0.965   0.895
#&gt; CV:kmeans   4 0.625           0.859       0.858         0.1630 0.965   0.895
#&gt; MAD:kmeans  4 0.859           0.890       0.895         0.1425 0.942   0.829
#&gt; ATC:kmeans  4 0.628           0.785       0.800         0.2326 0.802   0.586
#&gt; SD:pam      4 0.721           0.971       0.944         0.6540 0.704   0.515
#&gt; CV:pam      4 0.721           0.976       0.954         0.6772 0.704   0.515
#&gt; MAD:pam     4 1.000           0.930       0.964         0.0923 0.859   0.639
#&gt; ATC:pam     4 1.000           0.991       0.996         0.0494 0.979   0.961
#&gt; SD:hclust   4 0.912           0.975       0.986         0.2559 0.915   0.862
#&gt; CV:hclust   4 0.896           0.951       0.959         0.1994 0.915   0.862
#&gt; MAD:hclust  4 0.649           0.741       0.770         0.0669 0.979   0.940
#&gt; ATC:hclust  4 0.968           0.960       0.969         0.0455 0.979   0.961
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.761           0.757       0.880         0.0886 0.965   0.888
#&gt; CV:NMF      5 0.906           0.903       0.939         0.1153 0.838   0.594
#&gt; MAD:NMF     5 0.816           0.805       0.858         0.0654 0.855   0.589
#&gt; ATC:NMF     5 0.559           0.688       0.728         0.0843 0.962   0.891
#&gt; SD:skmeans  5 0.928           0.876       0.919         0.0315 0.993   0.973
#&gt; CV:skmeans  5 0.824           0.878       0.901         0.0488 0.972   0.906
#&gt; MAD:skmeans 5 0.844           0.826       0.868         0.0589 0.972   0.894
#&gt; ATC:skmeans 5 0.848           0.950       0.964         0.0490 0.972   0.906
#&gt; SD:mclust   5 0.730           0.831       0.911         0.0535 0.993   0.975
#&gt; CV:mclust   5 0.767           0.891       0.922         0.0314 0.993   0.975
#&gt; MAD:mclust  5 0.896           0.820       0.929         0.0372 0.965   0.879
#&gt; ATC:mclust  5 0.771           0.820       0.893         0.1528 0.761   0.473
#&gt; SD:kmeans   5 0.746           0.750       0.802         0.0990 1.000   1.000
#&gt; CV:kmeans   5 0.698           0.782       0.814         0.1203 1.000   1.000
#&gt; MAD:kmeans  5 0.704           0.771       0.825         0.0727 1.000   1.000
#&gt; ATC:kmeans  5 0.690           0.836       0.770         0.1081 0.914   0.692
#&gt; SD:pam      5 0.972           0.967       0.987         0.1176 0.965   0.888
#&gt; CV:pam      5 0.721           0.986       0.942         0.0514 0.965   0.888
#&gt; MAD:pam     5 1.000           0.932       0.971         0.0368 0.979   0.925
#&gt; ATC:pam     5 1.000           0.991       0.996         0.0295 0.986   0.973
#&gt; SD:hclust   5 0.824           0.948       0.969         0.0289 0.986   0.973
#&gt; CV:hclust   5 0.904           0.933       0.964         0.0792 0.986   0.973
#&gt; MAD:hclust  5 0.960           0.976       0.987         0.1942 0.915   0.743
#&gt; ATC:hclust  5 1.000           0.978       0.990         0.0290 0.986   0.973
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.751           0.778       0.864         0.0422 1.000   1.000
#&gt; CV:NMF      6 0.842           0.653       0.856         0.0594 0.989   0.961
#&gt; MAD:NMF     6 0.795           0.711       0.864         0.0360 0.984   0.939
#&gt; ATC:NMF     6 0.634           0.776       0.849         0.0864 0.846   0.587
#&gt; SD:skmeans  6 0.855           0.802       0.876         0.0414 0.929   0.766
#&gt; CV:skmeans  6 0.824           0.749       0.811         0.0529 0.929   0.741
#&gt; MAD:skmeans 6 0.818           0.699       0.789         0.0466 0.937   0.736
#&gt; ATC:skmeans 6 0.908           0.945       0.941         0.1001 0.914   0.684
#&gt; SD:mclust   6 0.969           0.910       0.956         0.0704 0.958   0.849
#&gt; CV:mclust   6 0.921           0.949       0.968         0.0620 0.958   0.849
#&gt; MAD:mclust  6 0.889           0.844       0.906         0.0424 0.970   0.886
#&gt; ATC:mclust  6 0.797           0.864       0.909         0.0163 0.979   0.923
#&gt; SD:kmeans   6 0.703           0.672       0.752         0.0481 0.958   0.859
#&gt; CV:kmeans   6 0.706           0.532       0.635         0.0471 0.873   0.578
#&gt; MAD:kmeans  6 0.740           0.661       0.755         0.0536 0.976   0.917
#&gt; ATC:kmeans  6 0.673           0.810       0.758         0.0534 0.993   0.964
#&gt; SD:pam      6 0.972           0.967       0.987         0.0196 0.986   0.950
#&gt; CV:pam      6 1.000           0.997       0.998         0.0709 0.986   0.950
#&gt; MAD:pam     6 0.816           0.758       0.884         0.0942 0.890   0.606
#&gt; ATC:pam     6 0.693           0.916       0.928         0.4452 0.753   0.518
#&gt; SD:hclust   6 0.713           0.886       0.887         0.1399 0.993   0.986
#&gt; CV:hclust   6 1.000           0.954       0.966         0.0412 0.993   0.986
#&gt; MAD:hclust  6 1.000           0.976       0.987         0.0229 0.993   0.971
#&gt; ATC:hclust  6 1.000           1.000       1.000         0.1304 0.922   0.849
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n cell.type(p) cell.line(p) other(p) k
#&gt; SD:NMF      51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:NMF      51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:NMF     48     6.02e-11     1.43e-06  0.00723 2
#&gt; ATC:NMF     51     1.46e-11     9.31e-07  0.00589 2
#&gt; SD:skmeans  51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:skmeans  51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:skmeans 51     7.71e-09     9.31e-07  0.00223 2
#&gt; ATC:skmeans 51     1.46e-11     9.31e-07  0.00589 2
#&gt; SD:mclust   51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:mclust   51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:mclust  51     1.46e-11     9.31e-07  0.00589 2
#&gt; ATC:mclust  51     1.46e-11     9.31e-07  0.00589 2
#&gt; SD:kmeans   51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:kmeans   51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:kmeans  51     1.46e-11     9.31e-07  0.00589 2
#&gt; ATC:kmeans  51     1.46e-11     9.31e-07  0.00589 2
#&gt; SD:pam      51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:pam      51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:pam     51     1.46e-11     9.31e-07  0.00589 2
#&gt; ATC:pam     51     1.46e-11     9.31e-07  0.00589 2
#&gt; SD:hclust   51     1.46e-11     9.31e-07  0.00589 2
#&gt; CV:hclust   51     1.46e-11     9.31e-07  0.00589 2
#&gt; MAD:hclust  51     1.46e-11     9.31e-07  0.00589 2
#&gt; ATC:hclust  51     1.46e-11     9.31e-07  0.00589 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n cell.type(p) cell.line(p) other(p) k
#&gt; SD:NMF      43     4.60e-10     6.55e-09 1.29e-05 3
#&gt; CV:NMF      50     1.39e-11     8.89e-10 2.44e-04 3
#&gt; MAD:NMF     50     1.39e-11     6.67e-10 1.12e-06 3
#&gt; ATC:NMF     45     1.69e-10     8.37e-09 2.79e-06 3
#&gt; SD:skmeans  51     8.42e-12     1.37e-11 1.03e-06 3
#&gt; CV:skmeans  51     8.42e-12     1.37e-11 1.03e-06 3
#&gt; MAD:skmeans 51     6.64e-09     3.77e-10 4.74e-07 3
#&gt; ATC:skmeans 51     8.42e-12     1.37e-11 2.17e-07 3
#&gt; SD:mclust   48     2.06e-09     1.43e-10 1.91e-06 3
#&gt; CV:mclust   51     5.44e-10     1.37e-11 6.36e-06 3
#&gt; MAD:mclust  51     8.42e-12     1.37e-11 1.03e-06 3
#&gt; ATC:mclust  51     8.42e-12     4.61e-09 4.66e-06 3
#&gt; SD:kmeans   45     1.69e-10     3.43e-10 1.16e-06 3
#&gt; CV:kmeans   51     8.42e-12     1.37e-11 1.03e-06 3
#&gt; MAD:kmeans  50     1.39e-11     6.67e-10 1.12e-06 3
#&gt; ATC:kmeans  39     3.40e-09     8.56e-09 2.66e-04 3
#&gt; SD:pam      51     8.42e-12     1.37e-11 3.29e-04 3
#&gt; CV:pam      51     8.42e-12     1.37e-11 3.29e-04 3
#&gt; MAD:pam     50     1.39e-11     1.89e-10 1.02e-06 3
#&gt; ATC:pam     51     8.42e-12     1.37e-11 2.17e-07 3
#&gt; SD:hclust   51     8.42e-12     1.37e-11 3.29e-04 3
#&gt; CV:hclust   51     8.42e-12     1.37e-11 3.29e-04 3
#&gt; MAD:hclust  45     1.69e-10     8.33e-09 7.28e-07 3
#&gt; ATC:hclust  51     8.42e-12     1.37e-11 2.17e-07 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n cell.type(p) cell.line(p) other(p) k
#&gt; SD:NMF      49     1.30e-10     2.33e-15 8.06e-08 4
#&gt; CV:NMF      42     4.01e-09     8.59e-13 1.50e-05 4
#&gt; MAD:NMF     46     5.67e-10     7.30e-14 8.69e-11 4
#&gt; ATC:NMF     44     2.79e-10     5.86e-07 1.08e-04 4
#&gt; SD:skmeans  48     2.13e-10     6.46e-12 1.83e-10 4
#&gt; CV:skmeans  48     2.13e-10     7.40e-15 2.97e-11 4
#&gt; MAD:skmeans 51     4.89e-11     6.96e-12 5.84e-10 4
#&gt; ATC:skmeans 51     4.89e-11     2.26e-16 4.62e-11 4
#&gt; SD:mclust   51     2.90e-09     2.26e-16 1.42e-09 4
#&gt; CV:mclust   51     2.90e-09     2.26e-16 1.42e-09 4
#&gt; MAD:mclust  51     4.89e-11     3.46e-13 3.99e-10 4
#&gt; ATC:mclust  37     4.60e-08     3.16e-11 6.99e-06 4
#&gt; SD:kmeans   45     9.25e-10     2.27e-13 1.65e-11 4
#&gt; CV:kmeans   48     2.13e-10     8.08e-16 8.70e-10 4
#&gt; MAD:kmeans  49     1.30e-10     5.62e-12 7.82e-10 4
#&gt; ATC:kmeans  45     9.25e-10     1.13e-10 1.23e-09 4
#&gt; SD:pam      51     4.89e-11     2.26e-16 8.17e-08 4
#&gt; CV:pam      51     4.89e-11     2.26e-16 8.17e-08 4
#&gt; MAD:pam     48     2.13e-10     9.62e-13 8.09e-10 4
#&gt; ATC:pam     51     4.89e-11     2.26e-16 1.89e-08 4
#&gt; SD:hclust   51     4.89e-11     2.26e-16 1.89e-08 4
#&gt; CV:hclust   51     4.89e-11     2.26e-16 1.89e-08 4
#&gt; MAD:hclust  45     9.25e-10     6.46e-13 6.72e-08 4
#&gt; ATC:hclust  51     4.89e-11     2.26e-16 1.89e-08 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n cell.type(p) cell.line(p) other(p) k
#&gt; SD:NMF      42     1.67e-08     2.21e-16 3.43e-11 5
#&gt; CV:NMF      50     3.61e-10     1.86e-20 7.55e-12 5
#&gt; MAD:NMF     48     9.44e-10     3.50e-16 1.08e-11 5
#&gt; ATC:NMF     42     4.01e-09     6.41e-09 3.52e-08 5
#&gt; SD:skmeans  48     9.44e-10     2.80e-14 1.72e-13 5
#&gt; CV:skmeans  51     2.23e-10     6.99e-16 5.39e-13 5
#&gt; MAD:skmeans 48     9.44e-10     1.36e-14 7.29e-13 5
#&gt; ATC:skmeans 51     2.23e-10     6.99e-16 5.39e-13 5
#&gt; SD:mclust   45     3.98e-09     2.28e-18 8.09e-11 5
#&gt; CV:mclust   51     2.23e-10     3.95e-21 4.56e-12 5
#&gt; MAD:mclust  45     3.98e-09     3.67e-14 2.17e-10 5
#&gt; ATC:mclust  49     5.84e-10     4.07e-14 1.80e-08 5
#&gt; SD:kmeans   45     9.25e-10     2.27e-13 1.65e-11 5
#&gt; CV:kmeans   51     4.89e-11     2.26e-16 4.62e-11 5
#&gt; MAD:kmeans  49     1.30e-10     5.62e-12 7.82e-10 5
#&gt; ATC:kmeans  51     2.23e-10     1.51e-13 1.34e-09 5
#&gt; SD:pam      51     2.23e-10     3.95e-21 4.56e-12 5
#&gt; CV:pam      51     2.23e-10     3.95e-21 4.56e-12 5
#&gt; MAD:pam     48     9.44e-10     5.17e-17 4.55e-11 5
#&gt; ATC:pam     51     2.23e-10     3.95e-21 2.43e-10 5
#&gt; SD:hclust   51     2.23e-10     3.95e-21 2.43e-10 5
#&gt; CV:hclust   51     2.23e-10     3.95e-21 8.09e-12 5
#&gt; MAD:hclust  51     2.23e-10     1.24e-16 5.19e-11 5
#&gt; ATC:hclust  51     2.23e-10     3.95e-21 2.43e-10 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n cell.type(p) cell.line(p) other(p) k
#&gt; SD:NMF      45     3.98e-09     2.28e-18 8.09e-11 6
#&gt; CV:NMF      36     7.49e-08     1.21e-11 7.41e-06 6
#&gt; MAD:NMF     41     2.69e-08     3.20e-12 1.25e-09 6
#&gt; ATC:NMF     47     3.48e-10     1.21e-11 1.38e-09 6
#&gt; SD:skmeans  48     9.44e-10     2.80e-14 1.72e-13 6
#&gt; CV:skmeans  48     3.55e-09     5.83e-20 6.46e-15 6
#&gt; MAD:skmeans 40     1.49e-07     4.75e-14 6.38e-11 6
#&gt; ATC:skmeans 51     8.65e-10     2.88e-16 1.93e-12 6
#&gt; SD:mclust   48     3.55e-09     4.10e-20 3.79e-12 6
#&gt; CV:mclust   51     8.65e-10     2.41e-22 1.91e-11 6
#&gt; MAD:mclust  48     3.55e-09     5.89e-17 3.79e-12 6
#&gt; ATC:mclust  48     3.55e-09     3.42e-20 3.32e-12 6
#&gt; SD:kmeans   45     3.98e-09     3.76e-17 3.04e-14 6
#&gt; CV:kmeans   34     7.45e-07     1.56e-13 1.60e-08 6
#&gt; MAD:kmeans  44     6.42e-09     1.74e-11 5.32e-11 6
#&gt; ATC:kmeans  48     3.55e-09     3.68e-19 5.53e-11 6
#&gt; SD:pam      51     8.65e-10     7.10e-26 1.98e-15 6
#&gt; CV:pam      51     8.65e-10     7.10e-26 1.98e-15 6
#&gt; MAD:pam     43     3.70e-08     6.65e-19 1.87e-11 6
#&gt; ATC:pam     51     8.65e-10     7.10e-26 6.30e-14 6
#&gt; SD:hclust   51     8.65e-10     7.10e-26 3.43e-13 6
#&gt; CV:hclust   51     8.65e-10     7.10e-26 3.43e-13 6
#&gt; MAD:hclust  51     8.65e-10     2.54e-19 7.24e-14 6
#&gt; ATC:hclust  51     8.65e-10     2.84e-21 6.17e-12 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           1.000       1.000         0.0575 0.979   0.967
#> 4 4 0.912           0.975       0.986         0.2559 0.915   0.862
#> 5 5 0.824           0.948       0.969         0.0289 0.986   0.973
#> 6 6 0.713           0.886       0.887         0.1399 0.993   0.986
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM520665     2       0          1  0  1  0
#&gt; GSM520666     2       0          1  0  1  0
#&gt; GSM520667     2       0          1  0  1  0
#&gt; GSM520704     3       0          1  0  0  1
#&gt; GSM520705     3       0          1  0  0  1
#&gt; GSM520711     3       0          1  0  0  1
#&gt; GSM520692     2       0          1  0  1  0
#&gt; GSM520693     2       0          1  0  1  0
#&gt; GSM520694     2       0          1  0  1  0
#&gt; GSM520689     2       0          1  0  1  0
#&gt; GSM520690     2       0          1  0  1  0
#&gt; GSM520691     2       0          1  0  1  0
#&gt; GSM520668     1       0          1  1  0  0
#&gt; GSM520669     1       0          1  1  0  0
#&gt; GSM520670     1       0          1  1  0  0
#&gt; GSM520713     1       0          1  1  0  0
#&gt; GSM520714     1       0          1  1  0  0
#&gt; GSM520715     1       0          1  1  0  0
#&gt; GSM520695     1       0          1  1  0  0
#&gt; GSM520696     1       0          1  1  0  0
#&gt; GSM520697     1       0          1  1  0  0
#&gt; GSM520709     1       0          1  1  0  0
#&gt; GSM520710     1       0          1  1  0  0
#&gt; GSM520712     1       0          1  1  0  0
#&gt; GSM520698     1       0          1  1  0  0
#&gt; GSM520699     1       0          1  1  0  0
#&gt; GSM520700     1       0          1  1  0  0
#&gt; GSM520701     1       0          1  1  0  0
#&gt; GSM520702     1       0          1  1  0  0
#&gt; GSM520703     1       0          1  1  0  0
#&gt; GSM520671     1       0          1  1  0  0
#&gt; GSM520672     1       0          1  1  0  0
#&gt; GSM520673     1       0          1  1  0  0
#&gt; GSM520681     1       0          1  1  0  0
#&gt; GSM520682     1       0          1  1  0  0
#&gt; GSM520680     1       0          1  1  0  0
#&gt; GSM520677     1       0          1  1  0  0
#&gt; GSM520678     1       0          1  1  0  0
#&gt; GSM520679     1       0          1  1  0  0
#&gt; GSM520674     1       0          1  1  0  0
#&gt; GSM520675     1       0          1  1  0  0
#&gt; GSM520676     1       0          1  1  0  0
#&gt; GSM520686     1       0          1  1  0  0
#&gt; GSM520687     1       0          1  1  0  0
#&gt; GSM520688     1       0          1  1  0  0
#&gt; GSM520683     1       0          1  1  0  0
#&gt; GSM520684     1       0          1  1  0  0
#&gt; GSM520685     1       0          1  1  0  0
#&gt; GSM520708     1       0          1  1  0  0
#&gt; GSM520706     1       0          1  1  0  0
#&gt; GSM520707     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3 p4
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520704     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520705     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520711     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520668     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520669     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520670     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520713     1   0.276      0.875 0.872  0 0.128  0
#&gt; GSM520714     1   0.276      0.875 0.872  0 0.128  0
#&gt; GSM520715     1   0.276      0.875 0.872  0 0.128  0
#&gt; GSM520695     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520696     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520697     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520709     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520710     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520712     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520698     1   0.241      0.898 0.896  0 0.104  0
#&gt; GSM520699     1   0.241      0.898 0.896  0 0.104  0
#&gt; GSM520700     1   0.241      0.898 0.896  0 0.104  0
#&gt; GSM520701     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520702     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520703     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520671     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520672     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520673     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520681     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520682     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520680     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520677     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520678     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520679     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520674     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520675     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520676     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520686     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520687     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520688     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520683     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520684     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520685     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520708     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520706     1   0.000      0.980 1.000  0 0.000  0
#&gt; GSM520707     1   0.000      0.980 1.000  0 0.000  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2   0.000      0.791 0.000 1.000 0.000 0.000  0
#&gt; GSM520666     2   0.000      0.791 0.000 1.000 0.000 0.000  0
#&gt; GSM520667     2   0.000      0.791 0.000 1.000 0.000 0.000  0
#&gt; GSM520704     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520705     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520711     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520692     2   0.377      0.748 0.296 0.704 0.000 0.000  0
#&gt; GSM520693     2   0.377      0.748 0.296 0.704 0.000 0.000  0
#&gt; GSM520694     2   0.377      0.748 0.296 0.704 0.000 0.000  0
#&gt; GSM520689     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520690     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520691     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520668     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520669     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520670     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520713     4   0.238      0.875 0.000 0.000 0.128 0.872  0
#&gt; GSM520714     4   0.238      0.875 0.000 0.000 0.128 0.872  0
#&gt; GSM520715     4   0.238      0.875 0.000 0.000 0.128 0.872  0
#&gt; GSM520695     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520696     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520697     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520709     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520710     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520712     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520698     4   0.207      0.898 0.000 0.000 0.104 0.896  0
#&gt; GSM520699     4   0.207      0.898 0.000 0.000 0.104 0.896  0
#&gt; GSM520700     4   0.207      0.898 0.000 0.000 0.104 0.896  0
#&gt; GSM520701     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520702     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520703     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520671     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520672     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520673     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520681     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520682     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520680     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520677     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520678     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520679     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520674     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520675     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520676     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520686     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520687     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520688     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520683     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520684     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520685     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520708     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520706     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520707     4   0.000      0.980 0.000 0.000 0.000 1.000  0
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2   0.242      1.000 0.000 0.844 0.000 0.000 0.000 0.156
#&gt; GSM520666     2   0.242      1.000 0.000 0.844 0.000 0.000 0.000 0.156
#&gt; GSM520667     2   0.242      1.000 0.000 0.844 0.000 0.000 0.000 0.156
#&gt; GSM520704     5   0.404      1.000 0.000 0.156 0.000 0.000 0.752 0.092
#&gt; GSM520705     5   0.404      1.000 0.000 0.156 0.000 0.000 0.752 0.092
#&gt; GSM520711     5   0.404      1.000 0.000 0.156 0.000 0.000 0.752 0.092
#&gt; GSM520692     6   0.305      1.000 0.000 0.000 0.000 0.236 0.000 0.764
#&gt; GSM520693     6   0.305      1.000 0.000 0.000 0.000 0.236 0.000 0.764
#&gt; GSM520694     6   0.305      1.000 0.000 0.000 0.000 0.236 0.000 0.764
#&gt; GSM520689     4   0.000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520690     4   0.000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520691     4   0.000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520668     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520669     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520670     3   0.000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520713     1   0.567      0.724 0.620 0.000 0.072 0.000 0.236 0.072
#&gt; GSM520714     1   0.567      0.724 0.620 0.000 0.072 0.000 0.236 0.072
#&gt; GSM520715     1   0.567      0.724 0.620 0.000 0.072 0.000 0.236 0.072
#&gt; GSM520695     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520696     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520697     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520709     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520710     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520712     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520698     1   0.363      0.777 0.788 0.000 0.000 0.000 0.068 0.144
#&gt; GSM520699     1   0.363      0.777 0.788 0.000 0.000 0.000 0.068 0.144
#&gt; GSM520700     1   0.363      0.777 0.788 0.000 0.000 0.000 0.068 0.144
#&gt; GSM520701     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520702     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520703     1   0.313      0.826 0.752 0.000 0.000 0.000 0.248 0.000
#&gt; GSM520671     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520672     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520673     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520681     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520682     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520680     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520677     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520678     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520679     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520674     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520675     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520676     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520686     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520684     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520685     1   0.000      0.874 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520708     1   0.273      0.839 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; GSM520706     1   0.273      0.839 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; GSM520707     1   0.273      0.839 0.808 0.000 0.000 0.000 0.192 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> SD:hclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:hclust 51     8.42e-12     1.37e-11 3.29e-04 3
#> SD:hclust 51     4.89e-11     2.26e-16 1.89e-08 4
#> SD:hclust 51     2.23e-10     3.95e-21 2.43e-10 5
#> SD:hclust 51     8.65e-10     7.10e-26 3.43e-13 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.601           0.819       0.845         0.6038 0.704   0.532
#> 4 4 0.596           0.791       0.836         0.1568 0.965   0.895
#> 5 5 0.746           0.750       0.802         0.0990 1.000   1.000
#> 6 6 0.703           0.672       0.752         0.0481 0.958   0.859
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM520666     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM520667     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM520704     2  0.3267      0.937 0.000 0.884 0.116
#&gt; GSM520705     2  0.3267      0.937 0.000 0.884 0.116
#&gt; GSM520711     2  0.3267      0.937 0.000 0.884 0.116
#&gt; GSM520692     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM520689     2  0.0237      0.976 0.000 0.996 0.004
#&gt; GSM520690     2  0.0237      0.976 0.000 0.996 0.004
#&gt; GSM520691     2  0.0237      0.976 0.000 0.996 0.004
#&gt; GSM520668     3  0.5988      0.460 0.368 0.000 0.632
#&gt; GSM520669     3  0.5988      0.460 0.368 0.000 0.632
#&gt; GSM520670     3  0.5988      0.460 0.368 0.000 0.632
#&gt; GSM520713     3  0.5178      0.741 0.256 0.000 0.744
#&gt; GSM520714     3  0.5178      0.741 0.256 0.000 0.744
#&gt; GSM520715     3  0.5178      0.741 0.256 0.000 0.744
#&gt; GSM520695     3  0.6244      0.722 0.440 0.000 0.560
#&gt; GSM520696     3  0.6244      0.722 0.440 0.000 0.560
#&gt; GSM520697     3  0.6244      0.722 0.440 0.000 0.560
#&gt; GSM520709     3  0.6260      0.712 0.448 0.000 0.552
#&gt; GSM520710     3  0.6260      0.712 0.448 0.000 0.552
#&gt; GSM520712     3  0.6260      0.712 0.448 0.000 0.552
#&gt; GSM520698     3  0.4842      0.724 0.224 0.000 0.776
#&gt; GSM520699     3  0.4842      0.724 0.224 0.000 0.776
#&gt; GSM520700     3  0.4842      0.724 0.224 0.000 0.776
#&gt; GSM520701     3  0.6252      0.723 0.444 0.000 0.556
#&gt; GSM520702     3  0.6244      0.726 0.440 0.000 0.560
#&gt; GSM520703     3  0.6244      0.726 0.440 0.000 0.560
#&gt; GSM520671     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520672     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520673     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520681     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520680     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520677     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520678     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520679     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520674     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520675     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520676     1  0.0237      0.928 0.996 0.000 0.004
#&gt; GSM520686     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.928 1.000 0.000 0.000
#&gt; GSM520683     1  0.0424      0.921 0.992 0.000 0.008
#&gt; GSM520684     1  0.0424      0.921 0.992 0.000 0.008
#&gt; GSM520685     1  0.0424      0.921 0.992 0.000 0.008
#&gt; GSM520708     1  0.5098      0.421 0.752 0.000 0.248
#&gt; GSM520706     1  0.5098      0.421 0.752 0.000 0.248
#&gt; GSM520707     1  0.5098      0.421 0.752 0.000 0.248
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.2256      0.915 0.000 0.924 0.056 0.020
#&gt; GSM520666     2  0.2256      0.915 0.000 0.924 0.056 0.020
#&gt; GSM520667     2  0.2256      0.915 0.000 0.924 0.056 0.020
#&gt; GSM520704     2  0.4399      0.843 0.000 0.768 0.212 0.020
#&gt; GSM520705     2  0.4399      0.843 0.000 0.768 0.212 0.020
#&gt; GSM520711     2  0.4464      0.843 0.000 0.768 0.208 0.024
#&gt; GSM520692     2  0.0188      0.924 0.000 0.996 0.000 0.004
#&gt; GSM520693     2  0.0188      0.924 0.000 0.996 0.000 0.004
#&gt; GSM520694     2  0.0188      0.924 0.000 0.996 0.000 0.004
#&gt; GSM520689     2  0.1520      0.918 0.000 0.956 0.020 0.024
#&gt; GSM520690     2  0.1520      0.918 0.000 0.956 0.020 0.024
#&gt; GSM520691     2  0.1520      0.918 0.000 0.956 0.020 0.024
#&gt; GSM520668     3  0.6674      1.000 0.116 0.000 0.584 0.300
#&gt; GSM520669     3  0.6674      1.000 0.116 0.000 0.584 0.300
#&gt; GSM520670     3  0.6674      1.000 0.116 0.000 0.584 0.300
#&gt; GSM520713     4  0.4547      0.703 0.104 0.000 0.092 0.804
#&gt; GSM520714     4  0.4547      0.703 0.104 0.000 0.092 0.804
#&gt; GSM520715     4  0.4547      0.703 0.104 0.000 0.092 0.804
#&gt; GSM520695     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520696     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520697     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520709     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520710     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520712     4  0.3444      0.816 0.184 0.000 0.000 0.816
#&gt; GSM520698     4  0.6015      0.286 0.080 0.000 0.268 0.652
#&gt; GSM520699     4  0.6015      0.286 0.080 0.000 0.268 0.652
#&gt; GSM520700     4  0.6015      0.286 0.080 0.000 0.268 0.652
#&gt; GSM520701     4  0.3402      0.814 0.164 0.000 0.004 0.832
#&gt; GSM520702     4  0.3402      0.814 0.164 0.000 0.004 0.832
#&gt; GSM520703     4  0.3402      0.814 0.164 0.000 0.004 0.832
#&gt; GSM520671     1  0.0707      0.869 0.980 0.000 0.020 0.000
#&gt; GSM520672     1  0.0707      0.869 0.980 0.000 0.020 0.000
#&gt; GSM520673     1  0.0707      0.869 0.980 0.000 0.020 0.000
#&gt; GSM520681     1  0.1406      0.867 0.960 0.000 0.024 0.016
#&gt; GSM520682     1  0.1406      0.867 0.960 0.000 0.024 0.016
#&gt; GSM520680     1  0.0657      0.869 0.984 0.000 0.012 0.004
#&gt; GSM520677     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520678     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520679     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520674     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520675     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520676     1  0.2214      0.856 0.928 0.000 0.028 0.044
#&gt; GSM520686     1  0.1211      0.866 0.960 0.000 0.040 0.000
#&gt; GSM520687     1  0.1211      0.866 0.960 0.000 0.040 0.000
#&gt; GSM520688     1  0.1211      0.866 0.960 0.000 0.040 0.000
#&gt; GSM520683     1  0.1940      0.847 0.924 0.000 0.076 0.000
#&gt; GSM520684     1  0.2081      0.846 0.916 0.000 0.084 0.000
#&gt; GSM520685     1  0.2081      0.846 0.916 0.000 0.084 0.000
#&gt; GSM520708     1  0.6764      0.257 0.556 0.000 0.112 0.332
#&gt; GSM520706     1  0.6764      0.257 0.556 0.000 0.112 0.332
#&gt; GSM520707     1  0.6764      0.257 0.556 0.000 0.112 0.332
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2  0.1617      0.906 0.000 0.948 0.020 0.012 NA
#&gt; GSM520666     2  0.1617      0.906 0.000 0.948 0.020 0.012 NA
#&gt; GSM520667     2  0.1617      0.906 0.000 0.948 0.020 0.012 NA
#&gt; GSM520704     2  0.4029      0.809 0.000 0.744 0.024 0.000 NA
#&gt; GSM520705     2  0.4029      0.809 0.000 0.744 0.024 0.000 NA
#&gt; GSM520711     2  0.4083      0.810 0.000 0.744 0.028 0.000 NA
#&gt; GSM520692     2  0.0162      0.909 0.000 0.996 0.000 0.004 NA
#&gt; GSM520693     2  0.0162      0.909 0.000 0.996 0.000 0.004 NA
#&gt; GSM520694     2  0.0162      0.909 0.000 0.996 0.000 0.004 NA
#&gt; GSM520689     2  0.2400      0.890 0.000 0.912 0.020 0.020 NA
#&gt; GSM520690     2  0.2400      0.890 0.000 0.912 0.020 0.020 NA
#&gt; GSM520691     2  0.2400      0.890 0.000 0.912 0.020 0.020 NA
#&gt; GSM520668     3  0.2248      0.998 0.012 0.000 0.900 0.088 NA
#&gt; GSM520669     3  0.2248      0.998 0.012 0.000 0.900 0.088 NA
#&gt; GSM520670     3  0.2532      0.995 0.012 0.000 0.892 0.088 NA
#&gt; GSM520713     4  0.4385      0.722 0.032 0.000 0.112 0.796 NA
#&gt; GSM520714     4  0.4385      0.722 0.032 0.000 0.112 0.796 NA
#&gt; GSM520715     4  0.4385      0.722 0.032 0.000 0.112 0.796 NA
#&gt; GSM520695     4  0.2504      0.801 0.064 0.000 0.000 0.896 NA
#&gt; GSM520696     4  0.2504      0.801 0.064 0.000 0.000 0.896 NA
#&gt; GSM520697     4  0.2504      0.801 0.064 0.000 0.000 0.896 NA
#&gt; GSM520709     4  0.1809      0.803 0.060 0.000 0.000 0.928 NA
#&gt; GSM520710     4  0.1809      0.803 0.060 0.000 0.000 0.928 NA
#&gt; GSM520712     4  0.1809      0.803 0.060 0.000 0.000 0.928 NA
#&gt; GSM520698     4  0.6640      0.275 0.028 0.000 0.352 0.500 NA
#&gt; GSM520699     4  0.6640      0.275 0.028 0.000 0.352 0.500 NA
#&gt; GSM520700     4  0.6640      0.275 0.028 0.000 0.352 0.500 NA
#&gt; GSM520701     4  0.2962      0.794 0.048 0.000 0.000 0.868 NA
#&gt; GSM520702     4  0.2962      0.794 0.048 0.000 0.000 0.868 NA
#&gt; GSM520703     4  0.2962      0.794 0.048 0.000 0.000 0.868 NA
#&gt; GSM520671     1  0.2825      0.766 0.860 0.000 0.016 0.000 NA
#&gt; GSM520672     1  0.2825      0.766 0.860 0.000 0.016 0.000 NA
#&gt; GSM520673     1  0.2825      0.766 0.860 0.000 0.016 0.000 NA
#&gt; GSM520681     1  0.2964      0.777 0.856 0.000 0.000 0.024 NA
#&gt; GSM520682     1  0.2964      0.777 0.856 0.000 0.000 0.024 NA
#&gt; GSM520680     1  0.1757      0.775 0.936 0.000 0.012 0.004 NA
#&gt; GSM520677     1  0.3357      0.758 0.852 0.000 0.008 0.048 NA
#&gt; GSM520678     1  0.3357      0.758 0.852 0.000 0.008 0.048 NA
#&gt; GSM520679     1  0.3357      0.758 0.852 0.000 0.008 0.048 NA
#&gt; GSM520674     1  0.3357      0.758 0.852 0.000 0.008 0.048 NA
#&gt; GSM520675     1  0.3412      0.759 0.848 0.000 0.008 0.048 NA
#&gt; GSM520676     1  0.3357      0.758 0.852 0.000 0.008 0.048 NA
#&gt; GSM520686     1  0.3318      0.766 0.800 0.000 0.008 0.000 NA
#&gt; GSM520687     1  0.3318      0.766 0.800 0.000 0.008 0.000 NA
#&gt; GSM520688     1  0.3318      0.766 0.800 0.000 0.008 0.000 NA
#&gt; GSM520683     1  0.3242      0.757 0.784 0.000 0.000 0.000 NA
#&gt; GSM520684     1  0.3452      0.755 0.756 0.000 0.000 0.000 NA
#&gt; GSM520685     1  0.3452      0.755 0.756 0.000 0.000 0.000 NA
#&gt; GSM520708     1  0.6960      0.267 0.372 0.000 0.008 0.248 NA
#&gt; GSM520706     1  0.6960      0.267 0.372 0.000 0.008 0.248 NA
#&gt; GSM520707     1  0.6960      0.267 0.372 0.000 0.008 0.248 NA
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM520665     2  0.3284      0.805 0.000 0.832 0.008 0.000 0.104 NA
#&gt; GSM520666     2  0.3284      0.805 0.000 0.832 0.008 0.000 0.104 NA
#&gt; GSM520667     2  0.3284      0.805 0.000 0.832 0.008 0.000 0.104 NA
#&gt; GSM520704     2  0.3838      0.641 0.000 0.552 0.000 0.000 0.000 NA
#&gt; GSM520705     2  0.3966      0.641 0.000 0.552 0.000 0.004 0.000 NA
#&gt; GSM520711     2  0.4274      0.641 0.000 0.552 0.000 0.004 0.012 NA
#&gt; GSM520692     2  0.0146      0.826 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520693     2  0.0146      0.826 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520694     2  0.0146      0.826 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520689     2  0.2813      0.807 0.000 0.880 0.024 0.012 0.068 NA
#&gt; GSM520690     2  0.2813      0.807 0.000 0.880 0.024 0.012 0.068 NA
#&gt; GSM520691     2  0.2813      0.807 0.000 0.880 0.024 0.012 0.068 NA
#&gt; GSM520668     3  0.1333      0.995 0.008 0.000 0.944 0.048 0.000 NA
#&gt; GSM520669     3  0.1333      0.995 0.008 0.000 0.944 0.048 0.000 NA
#&gt; GSM520670     3  0.1847      0.990 0.008 0.000 0.928 0.048 0.008 NA
#&gt; GSM520713     4  0.5423      0.615 0.012 0.000 0.124 0.700 0.084 NA
#&gt; GSM520714     4  0.5417      0.615 0.012 0.000 0.124 0.700 0.092 NA
#&gt; GSM520715     4  0.5417      0.615 0.012 0.000 0.124 0.700 0.092 NA
#&gt; GSM520695     4  0.2812      0.736 0.040 0.000 0.000 0.872 0.016 NA
#&gt; GSM520696     4  0.2812      0.736 0.040 0.000 0.000 0.872 0.016 NA
#&gt; GSM520697     4  0.2812      0.736 0.040 0.000 0.000 0.872 0.016 NA
#&gt; GSM520709     4  0.1500      0.735 0.052 0.000 0.000 0.936 0.012 NA
#&gt; GSM520710     4  0.1500      0.735 0.052 0.000 0.000 0.936 0.012 NA
#&gt; GSM520712     4  0.1398      0.735 0.052 0.000 0.000 0.940 0.008 NA
#&gt; GSM520698     4  0.7167      0.213 0.012 0.000 0.308 0.408 0.064 NA
#&gt; GSM520699     4  0.7167      0.213 0.012 0.000 0.308 0.408 0.064 NA
#&gt; GSM520700     4  0.7167      0.213 0.012 0.000 0.308 0.408 0.064 NA
#&gt; GSM520701     4  0.3077      0.725 0.024 0.000 0.004 0.864 0.044 NA
#&gt; GSM520702     4  0.3077      0.725 0.024 0.000 0.004 0.864 0.044 NA
#&gt; GSM520703     4  0.3077      0.725 0.024 0.000 0.004 0.864 0.044 NA
#&gt; GSM520671     1  0.5317      0.562 0.604 0.000 0.004 0.004 0.272 NA
#&gt; GSM520672     1  0.5317      0.562 0.604 0.000 0.004 0.004 0.272 NA
#&gt; GSM520673     1  0.5317      0.562 0.604 0.000 0.004 0.004 0.272 NA
#&gt; GSM520681     1  0.2858      0.618 0.864 0.000 0.008 0.004 0.096 NA
#&gt; GSM520682     1  0.2858      0.618 0.864 0.000 0.008 0.004 0.096 NA
#&gt; GSM520680     1  0.4479      0.620 0.728 0.000 0.008 0.004 0.180 NA
#&gt; GSM520677     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520678     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520679     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520674     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520675     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520676     1  0.0260      0.621 0.992 0.000 0.000 0.008 0.000 NA
#&gt; GSM520686     1  0.5250      0.505 0.528 0.000 0.008 0.004 0.396 NA
#&gt; GSM520687     1  0.5250      0.505 0.528 0.000 0.008 0.004 0.396 NA
#&gt; GSM520688     1  0.5250      0.505 0.528 0.000 0.008 0.004 0.396 NA
#&gt; GSM520683     1  0.3971      0.384 0.548 0.000 0.000 0.004 0.448 NA
#&gt; GSM520684     1  0.3997      0.401 0.508 0.000 0.000 0.004 0.488 NA
#&gt; GSM520685     1  0.3997      0.401 0.508 0.000 0.000 0.004 0.488 NA
#&gt; GSM520708     5  0.6887      0.997 0.272 0.000 0.000 0.204 0.448 NA
#&gt; GSM520706     5  0.6847      0.998 0.272 0.000 0.000 0.204 0.452 NA
#&gt; GSM520707     5  0.6847      0.998 0.272 0.000 0.000 0.204 0.452 NA
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> SD:kmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:kmeans 45     1.69e-10     3.43e-10 1.16e-06 3
#> SD:kmeans 45     9.25e-10     2.27e-13 1.65e-11 4
#> SD:kmeans 45     9.25e-10     2.27e-13 1.65e-11 5
#> SD:kmeans 45     3.98e-09     3.76e-17 3.04e-14 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.788           0.908       0.934         0.7176 0.704   0.532
#> 4 4 0.960           0.876       0.946         0.1416 0.873   0.652
#> 5 5 0.928           0.876       0.919         0.0315 0.993   0.973
#> 6 6 0.855           0.802       0.876         0.0414 0.929   0.766
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520668     3  0.0237      0.756 0.004  0 0.996
#&gt; GSM520669     3  0.0237      0.756 0.004  0 0.996
#&gt; GSM520670     3  0.0237      0.756 0.004  0 0.996
#&gt; GSM520713     3  0.1529      0.772 0.040  0 0.960
#&gt; GSM520714     3  0.1529      0.772 0.040  0 0.960
#&gt; GSM520715     3  0.1529      0.772 0.040  0 0.960
#&gt; GSM520695     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520696     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520697     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520709     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520710     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520712     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520698     3  0.0000      0.756 0.000  0 1.000
#&gt; GSM520699     3  0.0000      0.756 0.000  0 1.000
#&gt; GSM520700     3  0.0000      0.756 0.000  0 1.000
#&gt; GSM520701     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520702     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520703     3  0.5926      0.721 0.356  0 0.644
#&gt; GSM520671     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520672     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520673     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520681     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520682     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520680     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520677     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520678     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520679     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520674     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520675     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520676     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520686     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520687     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520688     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520683     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520684     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520685     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM520708     1  0.0237      0.995 0.996  0 0.004
#&gt; GSM520706     1  0.0237      0.995 0.996  0 0.004
#&gt; GSM520707     1  0.0237      0.995 0.996  0 0.004
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520704     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520705     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520711     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM520668     3   0.000      0.745 0.000  0 1.000 0.000
#&gt; GSM520669     3   0.000      0.745 0.000  0 1.000 0.000
#&gt; GSM520670     3   0.000      0.745 0.000  0 1.000 0.000
#&gt; GSM520713     4   0.179      0.771 0.000  0 0.068 0.932
#&gt; GSM520714     4   0.179      0.771 0.000  0 0.068 0.932
#&gt; GSM520715     4   0.179      0.771 0.000  0 0.068 0.932
#&gt; GSM520695     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520696     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520697     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520709     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520710     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520712     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520698     3   0.487      0.652 0.000  0 0.596 0.404
#&gt; GSM520699     3   0.487      0.652 0.000  0 0.596 0.404
#&gt; GSM520700     3   0.487      0.652 0.000  0 0.596 0.404
#&gt; GSM520701     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520702     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520703     4   0.000      0.810 0.000  0 0.000 1.000
#&gt; GSM520671     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520672     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520673     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520681     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520682     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520680     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520677     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520678     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520679     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520674     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520675     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520676     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520686     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520687     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520688     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520683     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520684     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520685     1   0.000      1.000 1.000  0 0.000 0.000
#&gt; GSM520708     4   0.495      0.302 0.444  0 0.000 0.556
#&gt; GSM520706     4   0.497      0.282 0.452  0 0.000 0.548
#&gt; GSM520707     4   0.497      0.282 0.452  0 0.000 0.548
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520704     2  0.2172      0.929 0.000 0.908 0.076 0.000 0.016
#&gt; GSM520705     2  0.2172      0.929 0.000 0.908 0.076 0.000 0.016
#&gt; GSM520711     2  0.2172      0.929 0.000 0.908 0.076 0.000 0.016
#&gt; GSM520692     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.2648      1.000 0.000 0.000 0.848 0.000 0.152
#&gt; GSM520669     3  0.2648      1.000 0.000 0.000 0.848 0.000 0.152
#&gt; GSM520670     3  0.2648      1.000 0.000 0.000 0.848 0.000 0.152
#&gt; GSM520713     4  0.2438      0.733 0.000 0.000 0.040 0.900 0.060
#&gt; GSM520714     4  0.2438      0.733 0.000 0.000 0.040 0.900 0.060
#&gt; GSM520715     4  0.2438      0.733 0.000 0.000 0.040 0.900 0.060
#&gt; GSM520695     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520696     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520697     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520709     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520710     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520712     4  0.0000      0.765 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520698     5  0.2471      1.000 0.000 0.000 0.000 0.136 0.864
#&gt; GSM520699     5  0.2471      1.000 0.000 0.000 0.000 0.136 0.864
#&gt; GSM520700     5  0.2471      1.000 0.000 0.000 0.000 0.136 0.864
#&gt; GSM520701     4  0.3165      0.685 0.000 0.000 0.036 0.848 0.116
#&gt; GSM520702     4  0.3165      0.685 0.000 0.000 0.036 0.848 0.116
#&gt; GSM520703     4  0.3165      0.685 0.000 0.000 0.036 0.848 0.116
#&gt; GSM520671     1  0.0000      0.975 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.975 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.975 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.1202      0.975 0.960 0.000 0.004 0.004 0.032
#&gt; GSM520682     1  0.1202      0.975 0.960 0.000 0.004 0.004 0.032
#&gt; GSM520680     1  0.0865      0.975 0.972 0.000 0.000 0.004 0.024
#&gt; GSM520677     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520678     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520679     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520674     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520675     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520676     1  0.1082      0.974 0.964 0.000 0.000 0.008 0.028
#&gt; GSM520686     1  0.0451      0.972 0.988 0.000 0.008 0.000 0.004
#&gt; GSM520687     1  0.0451      0.972 0.988 0.000 0.008 0.000 0.004
#&gt; GSM520688     1  0.0451      0.972 0.988 0.000 0.008 0.000 0.004
#&gt; GSM520683     1  0.1082      0.960 0.964 0.000 0.008 0.000 0.028
#&gt; GSM520684     1  0.1082      0.960 0.964 0.000 0.008 0.000 0.028
#&gt; GSM520685     1  0.1082      0.960 0.964 0.000 0.008 0.000 0.028
#&gt; GSM520708     4  0.6394      0.258 0.436 0.000 0.072 0.456 0.036
#&gt; GSM520706     4  0.6394      0.258 0.436 0.000 0.072 0.456 0.036
#&gt; GSM520707     4  0.6394      0.258 0.436 0.000 0.072 0.456 0.036
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM520665     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520666     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520667     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520704     2  0.3984      0.622 0.000 0.596 0.000 0.000 0.008 NA
#&gt; GSM520705     2  0.3984      0.622 0.000 0.596 0.000 0.000 0.008 NA
#&gt; GSM520711     2  0.3984      0.622 0.000 0.596 0.000 0.000 0.008 NA
#&gt; GSM520692     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520693     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520694     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520689     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520690     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520691     2  0.0000      0.894 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM520668     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM520669     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM520670     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM520713     4  0.3206      0.797 0.000 0.000 0.004 0.816 0.028 NA
#&gt; GSM520714     4  0.3206      0.797 0.000 0.000 0.004 0.816 0.028 NA
#&gt; GSM520715     4  0.3206      0.797 0.000 0.000 0.004 0.816 0.028 NA
#&gt; GSM520695     4  0.0000      0.872 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM520696     4  0.0000      0.872 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM520697     4  0.0000      0.872 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM520709     4  0.0363      0.872 0.000 0.000 0.000 0.988 0.000 NA
#&gt; GSM520710     4  0.0363      0.872 0.000 0.000 0.000 0.988 0.000 NA
#&gt; GSM520712     4  0.0363      0.872 0.000 0.000 0.000 0.988 0.000 NA
#&gt; GSM520698     5  0.1720      1.000 0.000 0.000 0.032 0.040 0.928 NA
#&gt; GSM520699     5  0.1720      1.000 0.000 0.000 0.032 0.040 0.928 NA
#&gt; GSM520700     5  0.1720      1.000 0.000 0.000 0.032 0.040 0.928 NA
#&gt; GSM520701     4  0.4079      0.726 0.000 0.000 0.000 0.744 0.172 NA
#&gt; GSM520702     4  0.4079      0.726 0.000 0.000 0.000 0.744 0.172 NA
#&gt; GSM520703     4  0.4079      0.726 0.000 0.000 0.000 0.744 0.172 NA
#&gt; GSM520671     1  0.0508      0.829 0.984 0.000 0.000 0.000 0.004 NA
#&gt; GSM520672     1  0.0508      0.829 0.984 0.000 0.000 0.000 0.004 NA
#&gt; GSM520673     1  0.0508      0.829 0.984 0.000 0.000 0.000 0.004 NA
#&gt; GSM520681     1  0.2799      0.824 0.852 0.000 0.000 0.012 0.012 NA
#&gt; GSM520682     1  0.2799      0.824 0.852 0.000 0.000 0.012 0.012 NA
#&gt; GSM520680     1  0.1442      0.829 0.944 0.000 0.000 0.012 0.004 NA
#&gt; GSM520677     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520678     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520679     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520674     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520675     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520676     1  0.2547      0.819 0.868 0.000 0.000 0.016 0.004 NA
#&gt; GSM520686     1  0.1500      0.823 0.936 0.000 0.000 0.000 0.012 NA
#&gt; GSM520687     1  0.1500      0.823 0.936 0.000 0.000 0.000 0.012 NA
#&gt; GSM520688     1  0.1500      0.823 0.936 0.000 0.000 0.000 0.012 NA
#&gt; GSM520683     1  0.2020      0.815 0.896 0.000 0.000 0.000 0.008 NA
#&gt; GSM520684     1  0.1757      0.815 0.916 0.000 0.000 0.000 0.008 NA
#&gt; GSM520685     1  0.1757      0.815 0.916 0.000 0.000 0.000 0.008 NA
#&gt; GSM520708     1  0.6486      0.138 0.376 0.000 0.000 0.256 0.020 NA
#&gt; GSM520706     1  0.6486      0.138 0.376 0.000 0.000 0.256 0.020 NA
#&gt; GSM520707     1  0.6486      0.138 0.376 0.000 0.000 0.256 0.020 NA
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> SD:skmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:skmeans 51     8.42e-12     1.37e-11 1.03e-06 3
#> SD:skmeans 48     2.13e-10     6.46e-12 1.83e-10 4
#> SD:skmeans 48     9.44e-10     2.80e-14 1.72e-13 5
#> SD:skmeans 48     9.44e-10     2.80e-14 1.72e-13 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           0.998       0.998         0.0598 0.979   0.967
#> 4 4 0.721           0.971       0.944         0.6540 0.704   0.515
#> 5 5 0.972           0.967       0.987         0.1176 0.965   0.888
#> 6 6 0.972           0.967       0.987         0.0196 0.986   0.950
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 5
```

There is also optional best $k$ = 2 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520704     3  0.0237      1.000 0.000 0.004 0.996
#&gt; GSM520705     3  0.0237      1.000 0.000 0.004 0.996
#&gt; GSM520711     3  0.0237      1.000 0.000 0.004 0.996
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520668     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520669     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520670     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520713     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520714     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520715     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520695     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520696     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520697     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520709     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520710     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520712     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520698     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520699     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520700     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520701     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520702     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520703     1  0.0237      0.998 0.996 0.000 0.004
#&gt; GSM520671     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520708     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.998 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4
#&gt; GSM520665     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520704     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520705     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520711     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520668     4   0.000      0.734 0.000  0  0 1.000
#&gt; GSM520669     4   0.000      0.734 0.000  0  0 1.000
#&gt; GSM520670     4   0.000      0.734 0.000  0  0 1.000
#&gt; GSM520713     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520714     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520715     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520695     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520696     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520697     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520709     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520710     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520712     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520698     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520699     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520700     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520701     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520702     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520703     4   0.353      0.954 0.192  0  0 0.808
#&gt; GSM520671     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520672     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520673     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520681     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520682     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520680     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520677     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520678     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520679     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520674     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520675     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520676     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520686     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520687     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520688     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520683     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520684     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520685     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520708     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520706     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520707     1   0.000      1.000 1.000  0  0 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4 p5
#&gt; GSM520665     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520666     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520667     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520704     5  0.0000      1.000 0.000  0  0 0.000  1
#&gt; GSM520705     5  0.0000      1.000 0.000  0  0 0.000  1
#&gt; GSM520711     5  0.0000      1.000 0.000  0  0 0.000  1
#&gt; GSM520692     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520689     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520690     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520691     2  0.0000      1.000 0.000  1  0 0.000  0
#&gt; GSM520668     3  0.0000      1.000 0.000  0  1 0.000  0
#&gt; GSM520669     3  0.0000      1.000 0.000  0  1 0.000  0
#&gt; GSM520670     3  0.0000      1.000 0.000  0  1 0.000  0
#&gt; GSM520713     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520714     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520715     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520695     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520696     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520697     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520709     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520710     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520712     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520698     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520699     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520700     4  0.3480      0.605 0.248  0  0 0.752  0
#&gt; GSM520701     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520702     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520703     4  0.0000      0.976 0.000  0  0 1.000  0
#&gt; GSM520671     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520672     1  0.0162      0.972 0.996  0  0 0.004  0
#&gt; GSM520673     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520681     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520682     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520680     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520677     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520678     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520679     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520674     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520675     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520676     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520686     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520687     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520688     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520683     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520684     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520685     1  0.0000      0.975 1.000  0  0 0.000  0
#&gt; GSM520708     1  0.2813      0.791 0.832  0  0 0.168  0
#&gt; GSM520706     1  0.2424      0.839 0.868  0  0 0.132  0
#&gt; GSM520707     1  0.2127      0.867 0.892  0  0 0.108  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4 p5 p6
#&gt; GSM520665     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520666     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520667     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520704     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520705     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520711     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520692     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520689     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520690     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520691     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520668     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520669     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520670     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520713     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520714     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520715     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520695     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520696     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520697     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520709     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520710     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520712     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520698     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520699     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520700     4  0.3126      0.605 0.248  0  0 0.752  0  0
#&gt; GSM520701     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520702     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520703     4  0.0000      0.976 0.000  0  0 1.000  0  0
#&gt; GSM520671     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520672     1  0.0146      0.972 0.996  0  0 0.004  0  0
#&gt; GSM520673     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520681     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520682     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520680     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520677     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520678     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520679     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520674     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520675     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520676     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520686     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520687     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520688     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520683     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520684     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520685     1  0.0000      0.975 1.000  0  0 0.000  0  0
#&gt; GSM520708     1  0.2527      0.791 0.832  0  0 0.168  0  0
#&gt; GSM520706     1  0.2178      0.839 0.868  0  0 0.132  0  0
#&gt; GSM520707     1  0.1910      0.867 0.892  0  0 0.108  0  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n cell.type(p) cell.line(p) other(p) k
#> SD:pam 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:pam 51     8.42e-12     1.37e-11 3.29e-04 3
#> SD:pam 51     4.89e-11     2.26e-16 8.17e-08 4
#> SD:pam 51     2.23e-10     3.95e-21 4.56e-12 5
#> SD:pam 51     8.65e-10     7.10e-26 1.98e-15 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.832           0.879       0.942         0.4017 0.834   0.742
#> 4 4 0.682           0.867       0.916         0.3283 0.756   0.522
#> 5 5 0.730           0.831       0.911         0.0535 0.993   0.975
#> 6 6 0.969           0.910       0.956         0.0704 0.958   0.849
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520704     3  0.4399      0.645 0.000 0.188 0.812
#&gt; GSM520705     3  0.4399      0.645 0.000 0.188 0.812
#&gt; GSM520711     3  0.4399      0.645 0.000 0.188 0.812
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520668     3  0.0424      0.720 0.008 0.000 0.992
#&gt; GSM520669     3  0.0424      0.720 0.008 0.000 0.992
#&gt; GSM520670     3  0.0424      0.720 0.008 0.000 0.992
#&gt; GSM520713     1  0.3192      0.896 0.888 0.000 0.112
#&gt; GSM520714     1  0.3192      0.896 0.888 0.000 0.112
#&gt; GSM520715     1  0.3192      0.896 0.888 0.000 0.112
#&gt; GSM520695     1  0.2448      0.918 0.924 0.000 0.076
#&gt; GSM520696     1  0.2537      0.917 0.920 0.000 0.080
#&gt; GSM520697     1  0.2448      0.918 0.924 0.000 0.076
#&gt; GSM520709     1  0.3482      0.880 0.872 0.000 0.128
#&gt; GSM520710     1  0.3551      0.876 0.868 0.000 0.132
#&gt; GSM520712     1  0.3619      0.873 0.864 0.000 0.136
#&gt; GSM520698     3  0.6062      0.325 0.384 0.000 0.616
#&gt; GSM520699     3  0.6062      0.325 0.384 0.000 0.616
#&gt; GSM520700     1  0.6126      0.369 0.600 0.000 0.400
#&gt; GSM520701     1  0.2356      0.919 0.928 0.000 0.072
#&gt; GSM520702     1  0.2796      0.911 0.908 0.000 0.092
#&gt; GSM520703     1  0.2711      0.913 0.912 0.000 0.088
#&gt; GSM520671     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520708     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.948 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.948 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520704     3  0.0000      0.673 0.000  0 1.000 0.000
#&gt; GSM520705     3  0.0000      0.673 0.000  0 1.000 0.000
#&gt; GSM520711     3  0.0000      0.673 0.000  0 1.000 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520668     3  0.4999      0.563 0.000  0 0.508 0.492
#&gt; GSM520669     3  0.4999      0.563 0.000  0 0.508 0.492
#&gt; GSM520670     3  0.4999      0.563 0.000  0 0.508 0.492
#&gt; GSM520713     4  0.1637      0.852 0.060  0 0.000 0.940
#&gt; GSM520714     4  0.1637      0.852 0.060  0 0.000 0.940
#&gt; GSM520715     4  0.1637      0.852 0.060  0 0.000 0.940
#&gt; GSM520695     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520696     4  0.3219      0.902 0.164  0 0.000 0.836
#&gt; GSM520697     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520709     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520710     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520712     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520698     4  0.0336      0.780 0.008  0 0.000 0.992
#&gt; GSM520699     4  0.0336      0.780 0.008  0 0.000 0.992
#&gt; GSM520700     4  0.0336      0.780 0.008  0 0.000 0.992
#&gt; GSM520701     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520702     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520703     4  0.3266      0.903 0.168  0 0.000 0.832
#&gt; GSM520671     1  0.0592      0.924 0.984  0 0.000 0.016
#&gt; GSM520672     1  0.3356      0.779 0.824  0 0.000 0.176
#&gt; GSM520673     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520681     1  0.0188      0.932 0.996  0 0.000 0.004
#&gt; GSM520682     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520680     1  0.2408      0.840 0.896  0 0.000 0.104
#&gt; GSM520677     1  0.0188      0.932 0.996  0 0.000 0.004
#&gt; GSM520678     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520679     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520674     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520675     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520676     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520686     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520687     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520688     1  0.0000      0.934 1.000  0 0.000 0.000
#&gt; GSM520683     1  0.0336      0.930 0.992  0 0.000 0.008
#&gt; GSM520684     1  0.0336      0.930 0.992  0 0.000 0.008
#&gt; GSM520685     1  0.0336      0.930 0.992  0 0.000 0.008
#&gt; GSM520708     1  0.4134      0.642 0.740  0 0.000 0.260
#&gt; GSM520706     1  0.4134      0.642 0.740  0 0.000 0.260
#&gt; GSM520707     1  0.4134      0.642 0.740  0 0.000 0.260
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520704     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520705     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520711     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520668     3  0.4101      1.000 0.000  0 0.628 0.372  0
#&gt; GSM520669     3  0.4101      1.000 0.000  0 0.628 0.372  0
#&gt; GSM520670     3  0.4101      1.000 0.000  0 0.628 0.372  0
#&gt; GSM520713     4  0.0000      0.422 0.000  0 0.000 1.000  0
#&gt; GSM520714     4  0.0000      0.422 0.000  0 0.000 1.000  0
#&gt; GSM520715     4  0.0000      0.422 0.000  0 0.000 1.000  0
#&gt; GSM520695     4  0.6074      0.740 0.128  0 0.372 0.500  0
#&gt; GSM520696     4  0.6074      0.740 0.128  0 0.372 0.500  0
#&gt; GSM520697     4  0.6074      0.740 0.128  0 0.372 0.500  0
#&gt; GSM520709     4  0.6100      0.739 0.132  0 0.368 0.500  0
#&gt; GSM520710     4  0.6100      0.739 0.132  0 0.368 0.500  0
#&gt; GSM520712     4  0.6100      0.739 0.132  0 0.368 0.500  0
#&gt; GSM520698     4  0.0955      0.381 0.004  0 0.028 0.968  0
#&gt; GSM520699     4  0.0955      0.381 0.004  0 0.028 0.968  0
#&gt; GSM520700     4  0.0955      0.381 0.004  0 0.028 0.968  0
#&gt; GSM520701     4  0.6100      0.738 0.132  0 0.368 0.500  0
#&gt; GSM520702     4  0.6074      0.740 0.128  0 0.372 0.500  0
#&gt; GSM520703     4  0.6074      0.740 0.128  0 0.372 0.500  0
#&gt; GSM520671     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520672     1  0.3239      0.792 0.852  0 0.068 0.080  0
#&gt; GSM520673     1  0.0162      0.933 0.996  0 0.004 0.000  0
#&gt; GSM520681     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520682     1  0.0162      0.933 0.996  0 0.004 0.000  0
#&gt; GSM520680     1  0.0162      0.932 0.996  0 0.000 0.004  0
#&gt; GSM520677     1  0.0162      0.933 0.996  0 0.004 0.000  0
#&gt; GSM520678     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520679     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520674     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520675     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520676     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520686     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520687     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520688     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520683     1  0.0000      0.934 1.000  0 0.000 0.000  0
#&gt; GSM520684     1  0.0671      0.923 0.980  0 0.016 0.004  0
#&gt; GSM520685     1  0.0162      0.933 0.996  0 0.004 0.000  0
#&gt; GSM520708     1  0.4229      0.550 0.704  0 0.020 0.276  0
#&gt; GSM520706     1  0.4229      0.550 0.704  0 0.020 0.276  0
#&gt; GSM520707     1  0.4229      0.550 0.704  0 0.020 0.276  0
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM520665     2  0.1007      0.971 0.000 0.956 0.044 0.000  0 0.000
#&gt; GSM520666     2  0.1007      0.971 0.000 0.956 0.044 0.000  0 0.000
#&gt; GSM520667     2  0.1007      0.971 0.000 0.956 0.044 0.000  0 0.000
#&gt; GSM520704     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520705     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520711     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520692     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520693     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520694     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520689     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520690     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520691     2  0.0000      0.986 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520668     3  0.1007      1.000 0.000 0.000 0.956 0.000  0 0.044
#&gt; GSM520669     3  0.1007      1.000 0.000 0.000 0.956 0.000  0 0.044
#&gt; GSM520670     3  0.1007      1.000 0.000 0.000 0.956 0.000  0 0.044
#&gt; GSM520713     6  0.1556      0.931 0.000 0.000 0.000 0.080  0 0.920
#&gt; GSM520714     6  0.1556      0.931 0.000 0.000 0.000 0.080  0 0.920
#&gt; GSM520715     6  0.1556      0.931 0.000 0.000 0.000 0.080  0 0.920
#&gt; GSM520695     4  0.1075      0.958 0.000 0.000 0.000 0.952  0 0.048
#&gt; GSM520696     4  0.1075      0.958 0.000 0.000 0.000 0.952  0 0.048
#&gt; GSM520697     4  0.1075      0.958 0.000 0.000 0.000 0.952  0 0.048
#&gt; GSM520709     4  0.0146      0.977 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520710     4  0.0146      0.977 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520712     4  0.0146      0.977 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520698     6  0.0000      0.930 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520699     6  0.0000      0.930 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520700     6  0.0000      0.930 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520701     4  0.0000      0.977 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520702     4  0.0000      0.977 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520703     4  0.0000      0.977 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520671     1  0.0260      0.908 0.992 0.000 0.000 0.000  0 0.008
#&gt; GSM520672     1  0.3210      0.733 0.804 0.000 0.000 0.168  0 0.028
#&gt; GSM520673     1  0.0405      0.908 0.988 0.000 0.000 0.004  0 0.008
#&gt; GSM520681     1  0.0260      0.910 0.992 0.000 0.000 0.008  0 0.000
#&gt; GSM520682     1  0.0260      0.910 0.992 0.000 0.000 0.008  0 0.000
#&gt; GSM520680     1  0.0146      0.910 0.996 0.000 0.000 0.004  0 0.000
#&gt; GSM520677     1  0.0260      0.910 0.992 0.000 0.000 0.008  0 0.000
#&gt; GSM520678     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520679     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520674     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520675     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520676     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520686     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520687     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520688     1  0.0000      0.911 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520683     1  0.0260      0.910 0.992 0.000 0.000 0.008  0 0.000
#&gt; GSM520684     1  0.0935      0.893 0.964 0.000 0.000 0.032  0 0.004
#&gt; GSM520685     1  0.0363      0.909 0.988 0.000 0.000 0.012  0 0.000
#&gt; GSM520708     1  0.3797      0.365 0.580 0.000 0.000 0.420  0 0.000
#&gt; GSM520706     1  0.3804      0.357 0.576 0.000 0.000 0.424  0 0.000
#&gt; GSM520707     1  0.3797      0.365 0.580 0.000 0.000 0.420  0 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> SD:mclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:mclust 48     2.06e-09     1.43e-10 1.91e-06 3
#> SD:mclust 51     2.90e-09     2.26e-16 1.42e-09 4
#> SD:mclust 45     3.98e-09     2.28e-18 8.09e-11 5
#> SD:mclust 48     3.55e-09     4.10e-20 3.79e-12 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.597           0.656       0.768         0.5062 0.746   0.599
#> 4 4 0.737           0.839       0.914         0.2006 0.824   0.595
#> 5 5 0.761           0.757       0.880         0.0886 0.965   0.888
#> 6 6 0.751           0.778       0.864         0.0422 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520666     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520667     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520704     2   0.536    0.84751 0.000 0.724 0.276
#&gt; GSM520705     2   0.536    0.84751 0.000 0.724 0.276
#&gt; GSM520711     2   0.536    0.84751 0.000 0.724 0.276
#&gt; GSM520692     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520693     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520694     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520689     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520690     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520691     2   0.000    0.95172 0.000 1.000 0.000
#&gt; GSM520668     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520669     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520670     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520713     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520714     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520715     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520695     1   0.624   -0.42978 0.560 0.000 0.440
#&gt; GSM520696     3   0.631    0.65392 0.488 0.000 0.512
#&gt; GSM520697     1   0.627   -0.49375 0.544 0.000 0.456
#&gt; GSM520709     1   0.614   -0.27711 0.596 0.000 0.404
#&gt; GSM520710     1   0.614   -0.27711 0.596 0.000 0.404
#&gt; GSM520712     1   0.614   -0.27711 0.596 0.000 0.404
#&gt; GSM520698     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520699     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520700     3   0.583    0.92107 0.340 0.000 0.660
#&gt; GSM520701     1   0.619   -0.34610 0.580 0.000 0.420
#&gt; GSM520702     3   0.630    0.69090 0.476 0.000 0.524
#&gt; GSM520703     3   0.629    0.72048 0.464 0.000 0.536
#&gt; GSM520671     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520672     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520673     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520681     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520682     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520680     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520677     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520678     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520679     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520674     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520675     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520676     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520686     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520687     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520688     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520683     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520684     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520685     1   0.000    0.76570 1.000 0.000 0.000
#&gt; GSM520708     1   0.610   -0.23139 0.608 0.000 0.392
#&gt; GSM520706     1   0.581    0.00515 0.664 0.000 0.336
#&gt; GSM520707     1   0.394    0.55038 0.844 0.000 0.156
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520704     3  0.2760      1.000 0.000 0.128 0.872 0.000
#&gt; GSM520705     3  0.2760      1.000 0.000 0.128 0.872 0.000
#&gt; GSM520711     3  0.2760      1.000 0.000 0.128 0.872 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520668     4  0.2319      0.776 0.036 0.000 0.040 0.924
#&gt; GSM520669     4  0.2411      0.774 0.040 0.000 0.040 0.920
#&gt; GSM520670     4  0.2319      0.776 0.036 0.000 0.040 0.924
#&gt; GSM520713     4  0.1042      0.788 0.008 0.000 0.020 0.972
#&gt; GSM520714     4  0.1042      0.788 0.008 0.000 0.020 0.972
#&gt; GSM520715     4  0.1042      0.788 0.008 0.000 0.020 0.972
#&gt; GSM520695     4  0.4428      0.720 0.276 0.000 0.004 0.720
#&gt; GSM520696     4  0.3494      0.789 0.172 0.000 0.004 0.824
#&gt; GSM520697     4  0.4343      0.733 0.264 0.000 0.004 0.732
#&gt; GSM520709     4  0.4889      0.597 0.360 0.000 0.004 0.636
#&gt; GSM520710     4  0.4905      0.590 0.364 0.000 0.004 0.632
#&gt; GSM520712     4  0.4872      0.604 0.356 0.000 0.004 0.640
#&gt; GSM520698     4  0.0927      0.788 0.008 0.000 0.016 0.976
#&gt; GSM520699     4  0.0927      0.788 0.008 0.000 0.016 0.976
#&gt; GSM520700     4  0.0927      0.788 0.008 0.000 0.016 0.976
#&gt; GSM520701     4  0.4283      0.741 0.256 0.000 0.004 0.740
#&gt; GSM520702     4  0.3208      0.796 0.148 0.000 0.004 0.848
#&gt; GSM520703     4  0.3208      0.796 0.148 0.000 0.004 0.848
#&gt; GSM520671     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.931 1.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.4999     -0.249 0.508 0.000 0.000 0.492
#&gt; GSM520706     1  0.4776      0.218 0.624 0.000 0.000 0.376
#&gt; GSM520707     1  0.3764      0.636 0.784 0.000 0.000 0.216
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2  0.0404      0.991 0.000 0.988 0.012 0.000  0
#&gt; GSM520666     2  0.0404      0.991 0.000 0.988 0.012 0.000  0
#&gt; GSM520667     2  0.0404      0.991 0.000 0.988 0.012 0.000  0
#&gt; GSM520704     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520705     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520711     5  0.0000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520692     2  0.0000      0.993 0.000 1.000 0.000 0.000  0
#&gt; GSM520693     2  0.0000      0.993 0.000 1.000 0.000 0.000  0
#&gt; GSM520694     2  0.0000      0.993 0.000 1.000 0.000 0.000  0
#&gt; GSM520689     2  0.0290      0.992 0.000 0.992 0.008 0.000  0
#&gt; GSM520690     2  0.0290      0.992 0.000 0.992 0.008 0.000  0
#&gt; GSM520691     2  0.0290      0.992 0.000 0.992 0.008 0.000  0
#&gt; GSM520668     3  0.3177      1.000 0.000 0.000 0.792 0.208  0
#&gt; GSM520669     3  0.3177      1.000 0.000 0.000 0.792 0.208  0
#&gt; GSM520670     3  0.3177      1.000 0.000 0.000 0.792 0.208  0
#&gt; GSM520713     4  0.3790      0.359 0.004 0.000 0.272 0.724  0
#&gt; GSM520714     4  0.3521      0.422 0.004 0.000 0.232 0.764  0
#&gt; GSM520715     4  0.3550      0.417 0.004 0.000 0.236 0.760  0
#&gt; GSM520695     4  0.2605      0.714 0.148 0.000 0.000 0.852  0
#&gt; GSM520696     4  0.2561      0.715 0.144 0.000 0.000 0.856  0
#&gt; GSM520697     4  0.2605      0.714 0.148 0.000 0.000 0.852  0
#&gt; GSM520709     4  0.2773      0.707 0.164 0.000 0.000 0.836  0
#&gt; GSM520710     4  0.2852      0.701 0.172 0.000 0.000 0.828  0
#&gt; GSM520712     4  0.2930      0.707 0.164 0.000 0.004 0.832  0
#&gt; GSM520698     4  0.4242     -0.266 0.000 0.000 0.428 0.572  0
#&gt; GSM520699     4  0.4268     -0.304 0.000 0.000 0.444 0.556  0
#&gt; GSM520700     4  0.4273     -0.315 0.000 0.000 0.448 0.552  0
#&gt; GSM520701     4  0.2707      0.711 0.132 0.000 0.008 0.860  0
#&gt; GSM520702     4  0.2351      0.693 0.088 0.000 0.016 0.896  0
#&gt; GSM520703     4  0.2351      0.693 0.088 0.000 0.016 0.896  0
#&gt; GSM520671     1  0.0451      0.903 0.988 0.000 0.008 0.004  0
#&gt; GSM520672     1  0.0671      0.897 0.980 0.000 0.016 0.004  0
#&gt; GSM520673     1  0.0451      0.903 0.988 0.000 0.008 0.004  0
#&gt; GSM520681     1  0.1270      0.891 0.948 0.000 0.000 0.052  0
#&gt; GSM520682     1  0.0963      0.901 0.964 0.000 0.000 0.036  0
#&gt; GSM520680     1  0.0451      0.903 0.988 0.000 0.008 0.004  0
#&gt; GSM520677     1  0.1478      0.879 0.936 0.000 0.000 0.064  0
#&gt; GSM520678     1  0.0880      0.903 0.968 0.000 0.000 0.032  0
#&gt; GSM520679     1  0.0880      0.903 0.968 0.000 0.000 0.032  0
#&gt; GSM520674     1  0.0609      0.907 0.980 0.000 0.000 0.020  0
#&gt; GSM520675     1  0.0609      0.907 0.980 0.000 0.000 0.020  0
#&gt; GSM520676     1  0.0510      0.907 0.984 0.000 0.000 0.016  0
#&gt; GSM520686     1  0.0000      0.908 1.000 0.000 0.000 0.000  0
#&gt; GSM520687     1  0.0000      0.908 1.000 0.000 0.000 0.000  0
#&gt; GSM520688     1  0.0000      0.908 1.000 0.000 0.000 0.000  0
#&gt; GSM520683     1  0.0290      0.908 0.992 0.000 0.000 0.008  0
#&gt; GSM520684     1  0.0000      0.908 1.000 0.000 0.000 0.000  0
#&gt; GSM520685     1  0.0000      0.908 1.000 0.000 0.000 0.000  0
#&gt; GSM520708     1  0.4249      0.199 0.568 0.000 0.000 0.432  0
#&gt; GSM520706     1  0.4235      0.225 0.576 0.000 0.000 0.424  0
#&gt; GSM520707     1  0.4114      0.355 0.624 0.000 0.000 0.376  0
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.2520      0.882 0.000 0.844 0.000 0.000 0.004 0.152
#&gt; GSM520666     2  0.2520      0.882 0.000 0.844 0.000 0.000 0.004 0.152
#&gt; GSM520667     2  0.2558      0.879 0.000 0.840 0.000 0.000 0.004 0.156
#&gt; GSM520704     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520705     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520711     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520692     2  0.0146      0.930 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520693     2  0.0146      0.930 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520694     2  0.0000      0.930 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.1075      0.922 0.000 0.952 0.000 0.000 0.000 0.048
#&gt; GSM520690     2  0.1075      0.922 0.000 0.952 0.000 0.000 0.000 0.048
#&gt; GSM520691     2  0.1007      0.923 0.000 0.956 0.000 0.000 0.000 0.044
#&gt; GSM520668     3  0.0937      1.000 0.000 0.000 0.960 0.040 0.000 0.000
#&gt; GSM520669     3  0.0937      1.000 0.000 0.000 0.960 0.040 0.000 0.000
#&gt; GSM520670     3  0.0937      1.000 0.000 0.000 0.960 0.040 0.000 0.000
#&gt; GSM520713     4  0.3944      0.209 0.000 0.000 0.428 0.568 0.000 0.004
#&gt; GSM520714     4  0.3890      0.259 0.000 0.000 0.400 0.596 0.000 0.004
#&gt; GSM520715     4  0.3890      0.259 0.000 0.000 0.400 0.596 0.000 0.004
#&gt; GSM520695     4  0.0937      0.687 0.040 0.000 0.000 0.960 0.000 0.000
#&gt; GSM520696     4  0.1010      0.686 0.036 0.000 0.004 0.960 0.000 0.000
#&gt; GSM520697     4  0.1007      0.687 0.044 0.000 0.000 0.956 0.000 0.000
#&gt; GSM520709     4  0.1686      0.682 0.064 0.000 0.000 0.924 0.000 0.012
#&gt; GSM520710     4  0.1686      0.682 0.064 0.000 0.000 0.924 0.000 0.012
#&gt; GSM520712     4  0.1584      0.680 0.064 0.000 0.000 0.928 0.000 0.008
#&gt; GSM520698     4  0.6005      0.216 0.004 0.000 0.356 0.436 0.000 0.204
#&gt; GSM520699     4  0.6013      0.201 0.004 0.000 0.364 0.428 0.000 0.204
#&gt; GSM520700     4  0.5991      0.180 0.004 0.000 0.380 0.420 0.000 0.196
#&gt; GSM520701     4  0.4121      0.636 0.056 0.000 0.004 0.732 0.000 0.208
#&gt; GSM520702     4  0.4143      0.638 0.052 0.000 0.008 0.736 0.000 0.204
#&gt; GSM520703     4  0.4114      0.640 0.052 0.000 0.008 0.740 0.000 0.200
#&gt; GSM520671     1  0.0363      0.898 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM520672     1  0.1074      0.887 0.960 0.000 0.012 0.028 0.000 0.000
#&gt; GSM520673     1  0.0363      0.898 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM520681     1  0.1930      0.901 0.916 0.000 0.000 0.036 0.000 0.048
#&gt; GSM520682     1  0.1257      0.904 0.952 0.000 0.000 0.020 0.000 0.028
#&gt; GSM520680     1  0.0260      0.899 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM520677     1  0.2668      0.817 0.828 0.000 0.000 0.168 0.000 0.004
#&gt; GSM520678     1  0.2320      0.851 0.864 0.000 0.000 0.132 0.000 0.004
#&gt; GSM520679     1  0.1958      0.875 0.896 0.000 0.000 0.100 0.000 0.004
#&gt; GSM520674     1  0.1908      0.874 0.900 0.000 0.000 0.096 0.000 0.004
#&gt; GSM520675     1  0.1471      0.890 0.932 0.000 0.000 0.064 0.000 0.004
#&gt; GSM520676     1  0.1753      0.879 0.912 0.000 0.000 0.084 0.000 0.004
#&gt; GSM520686     1  0.1267      0.899 0.940 0.000 0.000 0.000 0.000 0.060
#&gt; GSM520687     1  0.1204      0.899 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM520688     1  0.1204      0.899 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM520683     1  0.1500      0.901 0.936 0.000 0.000 0.012 0.000 0.052
#&gt; GSM520684     1  0.1349      0.900 0.940 0.000 0.000 0.004 0.000 0.056
#&gt; GSM520685     1  0.1524      0.900 0.932 0.000 0.000 0.008 0.000 0.060
#&gt; GSM520708     1  0.4429      0.723 0.716 0.000 0.000 0.144 0.000 0.140
#&gt; GSM520706     1  0.4425      0.723 0.716 0.000 0.000 0.152 0.000 0.132
#&gt; GSM520707     1  0.4273      0.742 0.732 0.000 0.000 0.148 0.000 0.120
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n cell.type(p) cell.line(p) other(p) k
#> SD:NMF 51     1.46e-11     9.31e-07 5.89e-03 2
#> SD:NMF 43     4.60e-10     6.55e-09 1.29e-05 3
#> SD:NMF 49     1.30e-10     2.33e-15 8.06e-08 4
#> SD:NMF 42     1.67e-08     2.21e-16 3.43e-11 5
#> SD:NMF 45     3.98e-09     2.28e-18 8.09e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           1.000       1.000         0.0575 0.979   0.967
#> 4 4 0.896           0.951       0.959         0.1994 0.915   0.862
#> 5 5 0.904           0.933       0.964         0.0792 0.986   0.973
#> 6 6 1.000           0.954       0.966         0.0412 0.993   0.986
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM520665     2       0          1  0  1  0
#&gt; GSM520666     2       0          1  0  1  0
#&gt; GSM520667     2       0          1  0  1  0
#&gt; GSM520704     3       0          1  0  0  1
#&gt; GSM520705     3       0          1  0  0  1
#&gt; GSM520711     3       0          1  0  0  1
#&gt; GSM520692     2       0          1  0  1  0
#&gt; GSM520693     2       0          1  0  1  0
#&gt; GSM520694     2       0          1  0  1  0
#&gt; GSM520689     2       0          1  0  1  0
#&gt; GSM520690     2       0          1  0  1  0
#&gt; GSM520691     2       0          1  0  1  0
#&gt; GSM520668     1       0          1  1  0  0
#&gt; GSM520669     1       0          1  1  0  0
#&gt; GSM520670     1       0          1  1  0  0
#&gt; GSM520713     1       0          1  1  0  0
#&gt; GSM520714     1       0          1  1  0  0
#&gt; GSM520715     1       0          1  1  0  0
#&gt; GSM520695     1       0          1  1  0  0
#&gt; GSM520696     1       0          1  1  0  0
#&gt; GSM520697     1       0          1  1  0  0
#&gt; GSM520709     1       0          1  1  0  0
#&gt; GSM520710     1       0          1  1  0  0
#&gt; GSM520712     1       0          1  1  0  0
#&gt; GSM520698     1       0          1  1  0  0
#&gt; GSM520699     1       0          1  1  0  0
#&gt; GSM520700     1       0          1  1  0  0
#&gt; GSM520701     1       0          1  1  0  0
#&gt; GSM520702     1       0          1  1  0  0
#&gt; GSM520703     1       0          1  1  0  0
#&gt; GSM520671     1       0          1  1  0  0
#&gt; GSM520672     1       0          1  1  0  0
#&gt; GSM520673     1       0          1  1  0  0
#&gt; GSM520681     1       0          1  1  0  0
#&gt; GSM520682     1       0          1  1  0  0
#&gt; GSM520680     1       0          1  1  0  0
#&gt; GSM520677     1       0          1  1  0  0
#&gt; GSM520678     1       0          1  1  0  0
#&gt; GSM520679     1       0          1  1  0  0
#&gt; GSM520674     1       0          1  1  0  0
#&gt; GSM520675     1       0          1  1  0  0
#&gt; GSM520676     1       0          1  1  0  0
#&gt; GSM520686     1       0          1  1  0  0
#&gt; GSM520687     1       0          1  1  0  0
#&gt; GSM520688     1       0          1  1  0  0
#&gt; GSM520683     1       0          1  1  0  0
#&gt; GSM520684     1       0          1  1  0  0
#&gt; GSM520685     1       0          1  1  0  0
#&gt; GSM520708     1       0          1  1  0  0
#&gt; GSM520706     1       0          1  1  0  0
#&gt; GSM520707     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3 p4
#&gt; GSM520665     2   0.194      0.894 0.000 0.924 0.076  0
#&gt; GSM520666     2   0.194      0.894 0.000 0.924 0.076  0
#&gt; GSM520667     2   0.194      0.894 0.000 0.924 0.076  0
#&gt; GSM520704     4   0.000      1.000 0.000 0.000 0.000  1
#&gt; GSM520705     4   0.000      1.000 0.000 0.000 0.000  1
#&gt; GSM520711     4   0.000      1.000 0.000 0.000 0.000  1
#&gt; GSM520692     2   0.000      0.909 0.000 1.000 0.000  0
#&gt; GSM520693     2   0.000      0.909 0.000 1.000 0.000  0
#&gt; GSM520694     2   0.000      0.909 0.000 1.000 0.000  0
#&gt; GSM520689     2   0.349      0.850 0.000 0.812 0.188  0
#&gt; GSM520690     2   0.349      0.850 0.000 0.812 0.188  0
#&gt; GSM520691     2   0.349      0.850 0.000 0.812 0.188  0
#&gt; GSM520668     3   0.416      1.000 0.264 0.000 0.736  0
#&gt; GSM520669     3   0.416      1.000 0.264 0.000 0.736  0
#&gt; GSM520670     3   0.416      1.000 0.264 0.000 0.736  0
#&gt; GSM520713     1   0.322      0.730 0.836 0.000 0.164  0
#&gt; GSM520714     1   0.322      0.730 0.836 0.000 0.164  0
#&gt; GSM520715     1   0.322      0.730 0.836 0.000 0.164  0
#&gt; GSM520695     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520696     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520697     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520709     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520710     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520712     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520698     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520699     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520700     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520701     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520702     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520703     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520671     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520672     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520673     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520681     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520682     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520680     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520677     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520678     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520679     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520674     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520675     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520676     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520686     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520687     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520688     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520683     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520684     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520685     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520708     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520706     1   0.000      0.980 1.000 0.000 0.000  0
#&gt; GSM520707     1   0.000      0.980 1.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520666     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520667     1   0.000      1.000 1.000 0.000 0.000 0.000  0
#&gt; GSM520704     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520705     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520711     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520692     2   0.414      0.626 0.384 0.616 0.000 0.000  0
#&gt; GSM520693     2   0.414      0.626 0.384 0.616 0.000 0.000  0
#&gt; GSM520694     2   0.414      0.626 0.384 0.616 0.000 0.000  0
#&gt; GSM520689     2   0.000      0.722 0.000 1.000 0.000 0.000  0
#&gt; GSM520690     2   0.000      0.722 0.000 1.000 0.000 0.000  0
#&gt; GSM520691     2   0.000      0.722 0.000 1.000 0.000 0.000  0
#&gt; GSM520668     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520669     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520670     3   0.000      1.000 0.000 0.000 1.000 0.000  0
#&gt; GSM520713     4   0.331      0.728 0.000 0.000 0.224 0.776  0
#&gt; GSM520714     4   0.331      0.728 0.000 0.000 0.224 0.776  0
#&gt; GSM520715     4   0.331      0.728 0.000 0.000 0.224 0.776  0
#&gt; GSM520695     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520696     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520697     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520709     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520710     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520712     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520698     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520699     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520700     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520701     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520702     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520703     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520671     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520672     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520673     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520681     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520682     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520680     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520677     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520678     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520679     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520674     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520675     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520676     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520686     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520687     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520688     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520683     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520684     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520685     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520708     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520706     4   0.000      0.980 0.000 0.000 0.000 1.000  0
#&gt; GSM520707     4   0.000      0.980 0.000 0.000 0.000 1.000  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2   p3    p4 p5    p6
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.00 0.000  0 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.00 0.000  0 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.00 0.000  0 0.000
#&gt; GSM520704     5   0.000      1.000 0.000  0 0.00 0.000  1 0.000
#&gt; GSM520705     5   0.000      1.000 0.000  0 0.00 0.000  1 0.000
#&gt; GSM520711     5   0.000      1.000 0.000  0 0.00 0.000  1 0.000
#&gt; GSM520692     6   0.144      1.000 0.000  0 0.00 0.072  0 0.928
#&gt; GSM520693     6   0.144      1.000 0.000  0 0.00 0.072  0 0.928
#&gt; GSM520694     6   0.144      1.000 0.000  0 0.00 0.072  0 0.928
#&gt; GSM520689     4   0.000      1.000 0.000  0 0.00 1.000  0 0.000
#&gt; GSM520690     4   0.000      1.000 0.000  0 0.00 1.000  0 0.000
#&gt; GSM520691     4   0.000      1.000 0.000  0 0.00 1.000  0 0.000
#&gt; GSM520668     3   0.000      1.000 0.000  0 1.00 0.000  0 0.000
#&gt; GSM520669     3   0.000      1.000 0.000  0 1.00 0.000  0 0.000
#&gt; GSM520670     3   0.000      1.000 0.000  0 1.00 0.000  0 0.000
#&gt; GSM520713     1   0.354      0.739 0.756  0 0.22 0.000  0 0.024
#&gt; GSM520714     1   0.354      0.739 0.756  0 0.22 0.000  0 0.024
#&gt; GSM520715     1   0.354      0.739 0.756  0 0.22 0.000  0 0.024
#&gt; GSM520695     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520696     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520697     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520709     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520710     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520712     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520698     1   0.133      0.943 0.936  0 0.00 0.000  0 0.064
#&gt; GSM520699     1   0.133      0.943 0.936  0 0.00 0.000  0 0.064
#&gt; GSM520700     1   0.133      0.943 0.936  0 0.00 0.000  0 0.064
#&gt; GSM520701     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520702     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520703     1   0.139      0.942 0.932  0 0.00 0.000  0 0.068
#&gt; GSM520671     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520672     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520673     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520681     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520682     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520680     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520677     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520678     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520679     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520674     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520675     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520676     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520686     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520687     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520688     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520683     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520684     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520685     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520708     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520706     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
#&gt; GSM520707     1   0.000      0.958 1.000  0 0.00 0.000  0 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> CV:hclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:hclust 51     8.42e-12     1.37e-11 3.29e-04 3
#> CV:hclust 51     4.89e-11     2.26e-16 1.89e-08 4
#> CV:hclust 51     2.23e-10     3.95e-21 8.09e-12 5
#> CV:hclust 51     8.65e-10     7.10e-26 3.43e-13 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.641           0.913       0.857         0.5624 0.704   0.532
#> 4 4 0.625           0.859       0.858         0.1630 0.965   0.895
#> 5 5 0.698           0.782       0.814         0.1203 1.000   1.000
#> 6 6 0.706           0.532       0.635         0.0471 0.873   0.578
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.1964      0.962 0.000 0.944 0.056
#&gt; GSM520666     2  0.1964      0.962 0.000 0.944 0.056
#&gt; GSM520667     2  0.1964      0.962 0.000 0.944 0.056
#&gt; GSM520704     2  0.3192      0.941 0.000 0.888 0.112
#&gt; GSM520705     2  0.3192      0.941 0.000 0.888 0.112
#&gt; GSM520711     2  0.3192      0.941 0.000 0.888 0.112
#&gt; GSM520692     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.971 0.000 1.000 0.000
#&gt; GSM520689     2  0.0237      0.971 0.000 0.996 0.004
#&gt; GSM520690     2  0.0237      0.971 0.000 0.996 0.004
#&gt; GSM520691     2  0.0237      0.971 0.000 0.996 0.004
#&gt; GSM520668     3  0.4002      0.700 0.160 0.000 0.840
#&gt; GSM520669     3  0.4002      0.700 0.160 0.000 0.840
#&gt; GSM520670     3  0.4002      0.700 0.160 0.000 0.840
#&gt; GSM520713     3  0.5706      0.811 0.320 0.000 0.680
#&gt; GSM520714     3  0.5706      0.811 0.320 0.000 0.680
#&gt; GSM520715     3  0.5706      0.811 0.320 0.000 0.680
#&gt; GSM520695     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520696     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520697     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520709     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520710     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520712     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520698     3  0.5882      0.822 0.348 0.000 0.652
#&gt; GSM520699     3  0.5882      0.822 0.348 0.000 0.652
#&gt; GSM520700     3  0.5882      0.822 0.348 0.000 0.652
#&gt; GSM520701     3  0.6302      0.804 0.480 0.000 0.520
#&gt; GSM520702     3  0.6274      0.816 0.456 0.000 0.544
#&gt; GSM520703     3  0.6274      0.816 0.456 0.000 0.544
#&gt; GSM520671     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520672     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520673     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520681     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520680     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520677     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520686     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520687     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520688     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520683     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520684     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520685     1  0.0592      0.988 0.988 0.000 0.012
#&gt; GSM520708     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.991 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.991 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.2089      0.921 0.000 0.932 0.048 0.020
#&gt; GSM520666     2  0.2089      0.921 0.000 0.932 0.048 0.020
#&gt; GSM520667     2  0.2089      0.921 0.000 0.932 0.048 0.020
#&gt; GSM520704     2  0.4406      0.845 0.000 0.780 0.192 0.028
#&gt; GSM520705     2  0.4459      0.846 0.000 0.780 0.188 0.032
#&gt; GSM520711     2  0.4459      0.846 0.000 0.780 0.188 0.032
#&gt; GSM520692     2  0.0000      0.930 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.930 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.930 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.1174      0.926 0.000 0.968 0.012 0.020
#&gt; GSM520690     2  0.1174      0.926 0.000 0.968 0.012 0.020
#&gt; GSM520691     2  0.1174      0.926 0.000 0.968 0.012 0.020
#&gt; GSM520668     3  0.5695      0.999 0.040 0.000 0.624 0.336
#&gt; GSM520669     3  0.5695      0.999 0.040 0.000 0.624 0.336
#&gt; GSM520670     3  0.5713      0.997 0.040 0.000 0.620 0.340
#&gt; GSM520713     4  0.5247      0.461 0.100 0.000 0.148 0.752
#&gt; GSM520714     4  0.5247      0.461 0.100 0.000 0.148 0.752
#&gt; GSM520715     4  0.5247      0.461 0.100 0.000 0.148 0.752
#&gt; GSM520695     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520696     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520697     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520709     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520710     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520712     4  0.3942      0.821 0.236 0.000 0.000 0.764
#&gt; GSM520698     4  0.6245      0.591 0.164 0.000 0.168 0.668
#&gt; GSM520699     4  0.6245      0.591 0.164 0.000 0.168 0.668
#&gt; GSM520700     4  0.6245      0.591 0.164 0.000 0.168 0.668
#&gt; GSM520701     4  0.3873      0.819 0.228 0.000 0.000 0.772
#&gt; GSM520702     4  0.3801      0.817 0.220 0.000 0.000 0.780
#&gt; GSM520703     4  0.3801      0.817 0.220 0.000 0.000 0.780
#&gt; GSM520671     1  0.0817      0.936 0.976 0.000 0.024 0.000
#&gt; GSM520672     1  0.0817      0.936 0.976 0.000 0.024 0.000
#&gt; GSM520673     1  0.0817      0.936 0.976 0.000 0.024 0.000
#&gt; GSM520681     1  0.1489      0.931 0.952 0.000 0.044 0.004
#&gt; GSM520682     1  0.1489      0.931 0.952 0.000 0.044 0.004
#&gt; GSM520680     1  0.1557      0.933 0.944 0.000 0.056 0.000
#&gt; GSM520677     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520678     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520679     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520674     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520675     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520676     1  0.3229      0.901 0.880 0.000 0.072 0.048
#&gt; GSM520686     1  0.1302      0.933 0.956 0.000 0.044 0.000
#&gt; GSM520687     1  0.1302      0.933 0.956 0.000 0.044 0.000
#&gt; GSM520688     1  0.1302      0.933 0.956 0.000 0.044 0.000
#&gt; GSM520683     1  0.1109      0.936 0.968 0.000 0.028 0.004
#&gt; GSM520684     1  0.1302      0.933 0.956 0.000 0.044 0.000
#&gt; GSM520685     1  0.1302      0.933 0.956 0.000 0.044 0.000
#&gt; GSM520708     1  0.1305      0.936 0.960 0.000 0.036 0.004
#&gt; GSM520706     1  0.1305      0.936 0.960 0.000 0.036 0.004
#&gt; GSM520707     1  0.1305      0.936 0.960 0.000 0.036 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2  0.2606      0.890 0.000 0.900 0.032 0.012 NA
#&gt; GSM520666     2  0.2606      0.890 0.000 0.900 0.032 0.012 NA
#&gt; GSM520667     2  0.2606      0.890 0.000 0.900 0.032 0.012 NA
#&gt; GSM520704     2  0.3912      0.812 0.000 0.752 0.020 0.000 NA
#&gt; GSM520705     2  0.3970      0.812 0.000 0.752 0.024 0.000 NA
#&gt; GSM520711     2  0.4148      0.812 0.000 0.752 0.028 0.004 NA
#&gt; GSM520692     2  0.0162      0.911 0.000 0.996 0.000 0.004 NA
#&gt; GSM520693     2  0.0162      0.911 0.000 0.996 0.000 0.004 NA
#&gt; GSM520694     2  0.0162      0.911 0.000 0.996 0.000 0.004 NA
#&gt; GSM520689     2  0.1278      0.907 0.000 0.960 0.020 0.004 NA
#&gt; GSM520690     2  0.1278      0.907 0.000 0.960 0.020 0.004 NA
#&gt; GSM520691     2  0.1278      0.907 0.000 0.960 0.020 0.004 NA
#&gt; GSM520668     3  0.2843      1.000 0.008 0.000 0.848 0.144 NA
#&gt; GSM520669     3  0.2843      1.000 0.008 0.000 0.848 0.144 NA
#&gt; GSM520670     3  0.2843      1.000 0.008 0.000 0.848 0.144 NA
#&gt; GSM520713     4  0.5355      0.545 0.028 0.000 0.216 0.692 NA
#&gt; GSM520714     4  0.5355      0.545 0.028 0.000 0.216 0.692 NA
#&gt; GSM520715     4  0.5355      0.545 0.028 0.000 0.216 0.692 NA
#&gt; GSM520695     4  0.1942      0.793 0.068 0.000 0.000 0.920 NA
#&gt; GSM520696     4  0.1942      0.793 0.068 0.000 0.000 0.920 NA
#&gt; GSM520697     4  0.1942      0.793 0.068 0.000 0.000 0.920 NA
#&gt; GSM520709     4  0.1942      0.792 0.068 0.000 0.000 0.920 NA
#&gt; GSM520710     4  0.1942      0.792 0.068 0.000 0.000 0.920 NA
#&gt; GSM520712     4  0.1942      0.792 0.068 0.000 0.000 0.920 NA
#&gt; GSM520698     4  0.5962      0.510 0.024 0.000 0.228 0.636 NA
#&gt; GSM520699     4  0.5962      0.510 0.024 0.000 0.228 0.636 NA
#&gt; GSM520700     4  0.5962      0.510 0.024 0.000 0.228 0.636 NA
#&gt; GSM520701     4  0.3266      0.777 0.056 0.000 0.008 0.860 NA
#&gt; GSM520702     4  0.3266      0.777 0.056 0.000 0.008 0.860 NA
#&gt; GSM520703     4  0.3266      0.777 0.056 0.000 0.008 0.860 NA
#&gt; GSM520671     1  0.3332      0.781 0.844 0.000 0.028 0.008 NA
#&gt; GSM520672     1  0.3332      0.781 0.844 0.000 0.028 0.008 NA
#&gt; GSM520673     1  0.3332      0.781 0.844 0.000 0.028 0.008 NA
#&gt; GSM520681     1  0.2886      0.788 0.844 0.000 0.008 0.000 NA
#&gt; GSM520682     1  0.2886      0.788 0.844 0.000 0.008 0.000 NA
#&gt; GSM520680     1  0.3421      0.787 0.788 0.000 0.008 0.000 NA
#&gt; GSM520677     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520678     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520679     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520674     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520675     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520676     1  0.4563      0.740 0.708 0.000 0.000 0.048 NA
#&gt; GSM520686     1  0.3550      0.761 0.796 0.000 0.020 0.000 NA
#&gt; GSM520687     1  0.3550      0.761 0.796 0.000 0.020 0.000 NA
#&gt; GSM520688     1  0.3550      0.761 0.796 0.000 0.020 0.000 NA
#&gt; GSM520683     1  0.3155      0.779 0.848 0.000 0.016 0.008 NA
#&gt; GSM520684     1  0.3873      0.753 0.768 0.000 0.012 0.008 NA
#&gt; GSM520685     1  0.3873      0.753 0.768 0.000 0.012 0.008 NA
#&gt; GSM520708     1  0.3247      0.780 0.840 0.000 0.016 0.008 NA
#&gt; GSM520706     1  0.3247      0.780 0.840 0.000 0.016 0.008 NA
#&gt; GSM520707     1  0.3247      0.780 0.840 0.000 0.016 0.008 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.3900      0.813 0.000 0.808 0.108 0.008 0.032 0.044
#&gt; GSM520666     2  0.3953      0.813 0.000 0.808 0.104 0.012 0.032 0.044
#&gt; GSM520667     2  0.3900      0.813 0.000 0.808 0.108 0.008 0.032 0.044
#&gt; GSM520704     2  0.5926      0.700 0.000 0.620 0.072 0.000 0.148 0.160
#&gt; GSM520705     2  0.5957      0.700 0.000 0.620 0.080 0.000 0.144 0.156
#&gt; GSM520711     2  0.5971      0.700 0.000 0.620 0.084 0.000 0.144 0.152
#&gt; GSM520692     2  0.0146      0.851 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520693     2  0.0146      0.851 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520694     2  0.0146      0.851 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520689     2  0.1890      0.845 0.000 0.924 0.024 0.008 0.044 0.000
#&gt; GSM520690     2  0.1890      0.845 0.000 0.924 0.024 0.008 0.044 0.000
#&gt; GSM520691     2  0.1890      0.845 0.000 0.924 0.024 0.008 0.044 0.000
#&gt; GSM520668     3  0.5336      0.995 0.004 0.000 0.596 0.260 0.000 0.140
#&gt; GSM520669     3  0.5336      0.995 0.004 0.000 0.596 0.260 0.000 0.140
#&gt; GSM520670     3  0.5635      0.990 0.004 0.000 0.580 0.260 0.008 0.148
#&gt; GSM520713     6  0.5336      0.526 0.024 0.000 0.044 0.328 0.012 0.592
#&gt; GSM520714     6  0.5336      0.526 0.024 0.000 0.044 0.328 0.012 0.592
#&gt; GSM520715     6  0.5336      0.526 0.024 0.000 0.044 0.328 0.012 0.592
#&gt; GSM520695     4  0.5227     -0.435 0.092 0.000 0.000 0.456 0.000 0.452
#&gt; GSM520696     4  0.5227     -0.435 0.092 0.000 0.000 0.456 0.000 0.452
#&gt; GSM520697     4  0.5227     -0.435 0.092 0.000 0.000 0.456 0.000 0.452
#&gt; GSM520709     6  0.5486      0.300 0.096 0.000 0.000 0.444 0.008 0.452
#&gt; GSM520710     6  0.5486      0.300 0.096 0.000 0.000 0.444 0.008 0.452
#&gt; GSM520712     6  0.5486      0.300 0.096 0.000 0.000 0.444 0.008 0.452
#&gt; GSM520698     4  0.1452      0.357 0.020 0.000 0.020 0.948 0.012 0.000
#&gt; GSM520699     4  0.1452      0.357 0.020 0.000 0.020 0.948 0.012 0.000
#&gt; GSM520700     4  0.1546      0.357 0.020 0.000 0.020 0.944 0.016 0.000
#&gt; GSM520701     4  0.4865      0.242 0.048 0.000 0.000 0.660 0.028 0.264
#&gt; GSM520702     4  0.4806      0.247 0.044 0.000 0.000 0.664 0.028 0.264
#&gt; GSM520703     4  0.4806      0.247 0.044 0.000 0.000 0.664 0.028 0.264
#&gt; GSM520671     1  0.6017     -0.122 0.484 0.000 0.124 0.000 0.364 0.028
#&gt; GSM520672     1  0.6017     -0.122 0.484 0.000 0.124 0.000 0.364 0.028
#&gt; GSM520673     1  0.6017     -0.122 0.484 0.000 0.124 0.000 0.364 0.028
#&gt; GSM520681     1  0.3663      0.476 0.792 0.000 0.020 0.000 0.160 0.028
#&gt; GSM520682     1  0.3663      0.476 0.792 0.000 0.020 0.000 0.160 0.028
#&gt; GSM520680     1  0.3513      0.506 0.804 0.000 0.024 0.000 0.152 0.020
#&gt; GSM520677     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520678     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520679     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520674     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520675     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520676     1  0.0405      0.672 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM520686     5  0.5951      0.651 0.412 0.000 0.064 0.000 0.464 0.060
#&gt; GSM520687     5  0.5951      0.651 0.412 0.000 0.064 0.000 0.464 0.060
#&gt; GSM520688     5  0.5951      0.651 0.412 0.000 0.064 0.000 0.464 0.060
#&gt; GSM520683     5  0.4630      0.756 0.404 0.000 0.008 0.000 0.560 0.028
#&gt; GSM520684     5  0.3887      0.765 0.360 0.000 0.000 0.000 0.632 0.008
#&gt; GSM520685     5  0.3887      0.765 0.360 0.000 0.000 0.000 0.632 0.008
#&gt; GSM520708     5  0.5066      0.733 0.424 0.000 0.012 0.004 0.520 0.040
#&gt; GSM520706     5  0.5066      0.733 0.424 0.000 0.012 0.004 0.520 0.040
#&gt; GSM520707     5  0.5066      0.733 0.424 0.000 0.012 0.004 0.520 0.040
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> CV:kmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:kmeans 51     8.42e-12     1.37e-11 1.03e-06 3
#> CV:kmeans 48     2.13e-10     8.08e-16 8.70e-10 4
#> CV:kmeans 51     4.89e-11     2.26e-16 4.62e-11 5
#> CV:kmeans 34     7.45e-07     1.56e-13 1.60e-08 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.991       0.995         0.3722 0.633   0.633
#> 3 3 0.788           0.963       0.971         0.7479 0.704   0.532
#> 4 4 0.968           0.913       0.961         0.0962 0.965   0.895
#> 5 5 0.824           0.878       0.901         0.0488 0.972   0.906
#> 6 6 0.824           0.749       0.811         0.0529 0.929   0.741
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette   p1   p2
#&gt; GSM520665     2   0.000      1.000 0.00 1.00
#&gt; GSM520666     2   0.000      1.000 0.00 1.00
#&gt; GSM520667     2   0.000      1.000 0.00 1.00
#&gt; GSM520704     2   0.000      1.000 0.00 1.00
#&gt; GSM520705     2   0.000      1.000 0.00 1.00
#&gt; GSM520711     2   0.000      1.000 0.00 1.00
#&gt; GSM520692     2   0.000      1.000 0.00 1.00
#&gt; GSM520693     2   0.000      1.000 0.00 1.00
#&gt; GSM520694     2   0.000      1.000 0.00 1.00
#&gt; GSM520689     2   0.000      1.000 0.00 1.00
#&gt; GSM520690     2   0.000      1.000 0.00 1.00
#&gt; GSM520691     2   0.000      1.000 0.00 1.00
#&gt; GSM520668     1   0.402      0.918 0.92 0.08
#&gt; GSM520669     1   0.402      0.918 0.92 0.08
#&gt; GSM520670     1   0.402      0.918 0.92 0.08
#&gt; GSM520713     1   0.000      0.994 1.00 0.00
#&gt; GSM520714     1   0.000      0.994 1.00 0.00
#&gt; GSM520715     1   0.000      0.994 1.00 0.00
#&gt; GSM520695     1   0.000      0.994 1.00 0.00
#&gt; GSM520696     1   0.000      0.994 1.00 0.00
#&gt; GSM520697     1   0.000      0.994 1.00 0.00
#&gt; GSM520709     1   0.000      0.994 1.00 0.00
#&gt; GSM520710     1   0.000      0.994 1.00 0.00
#&gt; GSM520712     1   0.000      0.994 1.00 0.00
#&gt; GSM520698     1   0.000      0.994 1.00 0.00
#&gt; GSM520699     1   0.000      0.994 1.00 0.00
#&gt; GSM520700     1   0.000      0.994 1.00 0.00
#&gt; GSM520701     1   0.000      0.994 1.00 0.00
#&gt; GSM520702     1   0.000      0.994 1.00 0.00
#&gt; GSM520703     1   0.000      0.994 1.00 0.00
#&gt; GSM520671     1   0.000      0.994 1.00 0.00
#&gt; GSM520672     1   0.000      0.994 1.00 0.00
#&gt; GSM520673     1   0.000      0.994 1.00 0.00
#&gt; GSM520681     1   0.000      0.994 1.00 0.00
#&gt; GSM520682     1   0.000      0.994 1.00 0.00
#&gt; GSM520680     1   0.000      0.994 1.00 0.00
#&gt; GSM520677     1   0.000      0.994 1.00 0.00
#&gt; GSM520678     1   0.000      0.994 1.00 0.00
#&gt; GSM520679     1   0.000      0.994 1.00 0.00
#&gt; GSM520674     1   0.000      0.994 1.00 0.00
#&gt; GSM520675     1   0.000      0.994 1.00 0.00
#&gt; GSM520676     1   0.000      0.994 1.00 0.00
#&gt; GSM520686     1   0.000      0.994 1.00 0.00
#&gt; GSM520687     1   0.000      0.994 1.00 0.00
#&gt; GSM520688     1   0.000      0.994 1.00 0.00
#&gt; GSM520683     1   0.000      0.994 1.00 0.00
#&gt; GSM520684     1   0.000      0.994 1.00 0.00
#&gt; GSM520685     1   0.000      0.994 1.00 0.00
#&gt; GSM520708     1   0.000      0.994 1.00 0.00
#&gt; GSM520706     1   0.000      0.994 1.00 0.00
#&gt; GSM520707     1   0.000      0.994 1.00 0.00
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM520668     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520669     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520670     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520713     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520714     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520715     3  0.0000      0.899 0.000  0 1.000
#&gt; GSM520695     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520696     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520697     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520709     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520710     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520712     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520698     3  0.0237      0.900 0.004  0 0.996
#&gt; GSM520699     3  0.0237      0.900 0.004  0 0.996
#&gt; GSM520700     3  0.0237      0.900 0.004  0 0.996
#&gt; GSM520701     3  0.4178      0.887 0.172  0 0.828
#&gt; GSM520702     3  0.3482      0.899 0.128  0 0.872
#&gt; GSM520703     3  0.3482      0.899 0.128  0 0.872
#&gt; GSM520671     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520672     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520673     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520681     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520682     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520680     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520677     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520678     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520679     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520674     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520675     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520676     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520686     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520687     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520688     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520683     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520684     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520685     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520708     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520706     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM520707     1  0.0000      1.000 1.000  0 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520668     3  0.0188      1.000 0.000  0 0.996 0.004
#&gt; GSM520669     3  0.0188      1.000 0.000  0 0.996 0.004
#&gt; GSM520670     3  0.0188      1.000 0.000  0 0.996 0.004
#&gt; GSM520713     4  0.4331      0.587 0.000  0 0.288 0.712
#&gt; GSM520714     4  0.4331      0.587 0.000  0 0.288 0.712
#&gt; GSM520715     4  0.4331      0.587 0.000  0 0.288 0.712
#&gt; GSM520695     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520696     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520697     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520709     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520710     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520712     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520698     4  0.4730      0.453 0.000  0 0.364 0.636
#&gt; GSM520699     4  0.4730      0.453 0.000  0 0.364 0.636
#&gt; GSM520700     4  0.4730      0.453 0.000  0 0.364 0.636
#&gt; GSM520701     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520702     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520703     4  0.0000      0.833 0.000  0 0.000 1.000
#&gt; GSM520671     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520672     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520673     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520681     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520682     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520680     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520677     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520678     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520679     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520674     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520675     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520676     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM520686     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520687     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520688     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520683     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520684     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520685     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520708     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520706     1  0.0188      0.998 0.996  0 0.004 0.000
#&gt; GSM520707     1  0.0188      0.998 0.996  0 0.004 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1   p2    p3    p4    p5
#&gt; GSM520665     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520704     2  0.2732      0.872 0.000 0.84 0.000 0.000 0.160
#&gt; GSM520705     2  0.2732      0.872 0.000 0.84 0.000 0.000 0.160
#&gt; GSM520711     2  0.2732      0.872 0.000 0.84 0.000 0.000 0.160
#&gt; GSM520692     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.960 0.000 1.00 0.000 0.000 0.000
#&gt; GSM520668     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000
#&gt; GSM520669     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000
#&gt; GSM520670     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000
#&gt; GSM520713     4  0.4065      0.660 0.000 0.00 0.180 0.772 0.048
#&gt; GSM520714     4  0.4065      0.660 0.000 0.00 0.180 0.772 0.048
#&gt; GSM520715     4  0.4065      0.660 0.000 0.00 0.180 0.772 0.048
#&gt; GSM520695     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520696     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520697     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520709     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520710     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520712     4  0.0000      0.820 0.000 0.00 0.000 1.000 0.000
#&gt; GSM520698     5  0.5938      1.000 0.000 0.00 0.128 0.320 0.552
#&gt; GSM520699     5  0.5938      1.000 0.000 0.00 0.128 0.320 0.552
#&gt; GSM520700     5  0.5938      1.000 0.000 0.00 0.128 0.320 0.552
#&gt; GSM520701     4  0.2891      0.641 0.000 0.00 0.000 0.824 0.176
#&gt; GSM520702     4  0.2891      0.641 0.000 0.00 0.000 0.824 0.176
#&gt; GSM520703     4  0.2891      0.641 0.000 0.00 0.000 0.824 0.176
#&gt; GSM520671     1  0.0880      0.906 0.968 0.00 0.000 0.000 0.032
#&gt; GSM520672     1  0.0880      0.906 0.968 0.00 0.000 0.000 0.032
#&gt; GSM520673     1  0.0880      0.906 0.968 0.00 0.000 0.000 0.032
#&gt; GSM520681     1  0.2329      0.889 0.876 0.00 0.000 0.000 0.124
#&gt; GSM520682     1  0.2329      0.889 0.876 0.00 0.000 0.000 0.124
#&gt; GSM520680     1  0.2690      0.886 0.844 0.00 0.000 0.000 0.156
#&gt; GSM520677     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520678     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520679     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520674     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520675     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520676     1  0.3282      0.866 0.804 0.00 0.000 0.008 0.188
#&gt; GSM520686     1  0.1043      0.904 0.960 0.00 0.000 0.000 0.040
#&gt; GSM520687     1  0.1043      0.904 0.960 0.00 0.000 0.000 0.040
#&gt; GSM520688     1  0.1043      0.904 0.960 0.00 0.000 0.000 0.040
#&gt; GSM520683     1  0.0880      0.905 0.968 0.00 0.000 0.000 0.032
#&gt; GSM520684     1  0.1121      0.903 0.956 0.00 0.000 0.000 0.044
#&gt; GSM520685     1  0.1121      0.903 0.956 0.00 0.000 0.000 0.044
#&gt; GSM520708     1  0.0963      0.906 0.964 0.00 0.000 0.000 0.036
#&gt; GSM520706     1  0.0963      0.906 0.964 0.00 0.000 0.000 0.036
#&gt; GSM520707     1  0.0963      0.906 0.964 0.00 0.000 0.000 0.036
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520704     2  0.4720      0.655 0.000 0.624 0.000 0.000 0.072 0.304
#&gt; GSM520705     2  0.4720      0.655 0.000 0.624 0.000 0.000 0.072 0.304
#&gt; GSM520711     2  0.4720      0.655 0.000 0.624 0.000 0.000 0.072 0.304
#&gt; GSM520692     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.902 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520669     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520670     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520713     4  0.5411      0.636 0.000 0.000 0.092 0.684 0.108 0.116
#&gt; GSM520714     4  0.5411      0.636 0.000 0.000 0.092 0.684 0.108 0.116
#&gt; GSM520715     4  0.5411      0.636 0.000 0.000 0.092 0.684 0.108 0.116
#&gt; GSM520695     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520696     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520697     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520709     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520710     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520712     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520698     5  0.3352      1.000 0.000 0.000 0.032 0.176 0.792 0.000
#&gt; GSM520699     5  0.3352      1.000 0.000 0.000 0.032 0.176 0.792 0.000
#&gt; GSM520700     5  0.3352      1.000 0.000 0.000 0.032 0.176 0.792 0.000
#&gt; GSM520701     4  0.3245      0.613 0.000 0.000 0.000 0.764 0.228 0.008
#&gt; GSM520702     4  0.3245      0.613 0.000 0.000 0.000 0.764 0.228 0.008
#&gt; GSM520703     4  0.3245      0.613 0.000 0.000 0.000 0.764 0.228 0.008
#&gt; GSM520671     1  0.3017      0.590 0.816 0.000 0.000 0.000 0.020 0.164
#&gt; GSM520672     1  0.3017      0.590 0.816 0.000 0.000 0.000 0.020 0.164
#&gt; GSM520673     1  0.3017      0.590 0.816 0.000 0.000 0.000 0.020 0.164
#&gt; GSM520681     1  0.3695     -0.299 0.624 0.000 0.000 0.000 0.000 0.376
#&gt; GSM520682     1  0.3695     -0.299 0.624 0.000 0.000 0.000 0.000 0.376
#&gt; GSM520680     1  0.3695     -0.333 0.624 0.000 0.000 0.000 0.000 0.376
#&gt; GSM520677     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520678     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520679     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520674     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520675     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520676     6  0.4218      1.000 0.428 0.000 0.000 0.016 0.000 0.556
#&gt; GSM520686     1  0.0260      0.750 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM520687     1  0.0260      0.750 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM520688     1  0.0260      0.750 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM520683     1  0.1152      0.741 0.952 0.000 0.000 0.000 0.004 0.044
#&gt; GSM520684     1  0.0146      0.747 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM520685     1  0.0291      0.748 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; GSM520708     1  0.1333      0.738 0.944 0.000 0.000 0.000 0.008 0.048
#&gt; GSM520706     1  0.1333      0.738 0.944 0.000 0.000 0.000 0.008 0.048
#&gt; GSM520707     1  0.1333      0.738 0.944 0.000 0.000 0.000 0.008 0.048
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> CV:skmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:skmeans 51     8.42e-12     1.37e-11 1.03e-06 3
#> CV:skmeans 48     2.13e-10     7.40e-15 2.97e-11 4
#> CV:skmeans 51     2.23e-10     6.99e-16 5.39e-13 5
#> CV:skmeans 48     3.55e-09     5.83e-20 6.46e-15 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           1.000       1.000         0.0575 0.979   0.967
#> 4 4 0.721           0.976       0.954         0.6772 0.704   0.515
#> 5 5 0.721           0.986       0.942         0.0514 0.965   0.888
#> 6 6 1.000           0.997       0.998         0.0709 0.986   0.950
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM520665     2       0          1  0  1  0
#&gt; GSM520666     2       0          1  0  1  0
#&gt; GSM520667     2       0          1  0  1  0
#&gt; GSM520704     3       0          1  0  0  1
#&gt; GSM520705     3       0          1  0  0  1
#&gt; GSM520711     3       0          1  0  0  1
#&gt; GSM520692     2       0          1  0  1  0
#&gt; GSM520693     2       0          1  0  1  0
#&gt; GSM520694     2       0          1  0  1  0
#&gt; GSM520689     2       0          1  0  1  0
#&gt; GSM520690     2       0          1  0  1  0
#&gt; GSM520691     2       0          1  0  1  0
#&gt; GSM520668     1       0          1  1  0  0
#&gt; GSM520669     1       0          1  1  0  0
#&gt; GSM520670     1       0          1  1  0  0
#&gt; GSM520713     1       0          1  1  0  0
#&gt; GSM520714     1       0          1  1  0  0
#&gt; GSM520715     1       0          1  1  0  0
#&gt; GSM520695     1       0          1  1  0  0
#&gt; GSM520696     1       0          1  1  0  0
#&gt; GSM520697     1       0          1  1  0  0
#&gt; GSM520709     1       0          1  1  0  0
#&gt; GSM520710     1       0          1  1  0  0
#&gt; GSM520712     1       0          1  1  0  0
#&gt; GSM520698     1       0          1  1  0  0
#&gt; GSM520699     1       0          1  1  0  0
#&gt; GSM520700     1       0          1  1  0  0
#&gt; GSM520701     1       0          1  1  0  0
#&gt; GSM520702     1       0          1  1  0  0
#&gt; GSM520703     1       0          1  1  0  0
#&gt; GSM520671     1       0          1  1  0  0
#&gt; GSM520672     1       0          1  1  0  0
#&gt; GSM520673     1       0          1  1  0  0
#&gt; GSM520681     1       0          1  1  0  0
#&gt; GSM520682     1       0          1  1  0  0
#&gt; GSM520680     1       0          1  1  0  0
#&gt; GSM520677     1       0          1  1  0  0
#&gt; GSM520678     1       0          1  1  0  0
#&gt; GSM520679     1       0          1  1  0  0
#&gt; GSM520674     1       0          1  1  0  0
#&gt; GSM520675     1       0          1  1  0  0
#&gt; GSM520676     1       0          1  1  0  0
#&gt; GSM520686     1       0          1  1  0  0
#&gt; GSM520687     1       0          1  1  0  0
#&gt; GSM520688     1       0          1  1  0  0
#&gt; GSM520683     1       0          1  1  0  0
#&gt; GSM520684     1       0          1  1  0  0
#&gt; GSM520685     1       0          1  1  0  0
#&gt; GSM520708     1       0          1  1  0  0
#&gt; GSM520706     1       0          1  1  0  0
#&gt; GSM520707     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4
#&gt; GSM520665     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520704     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520705     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520711     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM520668     4   0.000      0.790 0.000  0  0 1.000
#&gt; GSM520669     4   0.000      0.790 0.000  0  0 1.000
#&gt; GSM520670     4   0.000      0.790 0.000  0  0 1.000
#&gt; GSM520713     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520714     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520715     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520695     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520696     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520697     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520709     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520710     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520712     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520698     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520699     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520700     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520701     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520702     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520703     4   0.312      0.962 0.156  0  0 0.844
#&gt; GSM520671     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520672     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520673     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520681     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520682     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520680     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520677     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520678     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520679     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520674     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520675     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520676     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520686     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520687     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520688     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520683     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520684     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520685     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520708     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520706     1   0.000      1.000 1.000  0  0 0.000
#&gt; GSM520707     1   0.000      1.000 1.000  0  0 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2   0.310      0.884 0.148 0.836 0.016 0.000  0
#&gt; GSM520666     2   0.310      0.884 0.148 0.836 0.016 0.000  0
#&gt; GSM520667     2   0.310      0.884 0.148 0.836 0.016 0.000  0
#&gt; GSM520704     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520705     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520711     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; GSM520692     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520693     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520694     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520689     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520690     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520691     2   0.000      0.945 0.000 1.000 0.000 0.000  0
#&gt; GSM520668     3   0.051      1.000 0.000 0.000 0.984 0.016  0
#&gt; GSM520669     3   0.051      1.000 0.000 0.000 0.984 0.016  0
#&gt; GSM520670     3   0.051      1.000 0.000 0.000 0.984 0.016  0
#&gt; GSM520713     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520714     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520715     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520695     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520696     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520697     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520709     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520710     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520712     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520698     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520699     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520700     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520701     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520702     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520703     4   0.000      1.000 0.000 0.000 0.000 1.000  0
#&gt; GSM520671     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520672     1   0.265      0.995 0.848 0.000 0.000 0.152  0
#&gt; GSM520673     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520681     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520682     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520680     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520677     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520678     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520679     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520674     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520675     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520676     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520686     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520687     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520688     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520683     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520684     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520685     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520708     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520706     1   0.260      1.000 0.852 0.000 0.000 0.148  0
#&gt; GSM520707     1   0.260      1.000 0.852 0.000 0.000 0.148  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4 p5 p6
#&gt; GSM520665     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520666     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520667     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520704     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520705     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520711     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520692     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520689     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520690     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520691     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520668     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520669     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520670     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520713     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520714     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520715     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520695     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520696     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520697     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520709     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520710     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520712     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520698     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520699     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520700     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520701     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520702     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520703     4  0.0000      1.000 0.000  0  0 1.000  0  0
#&gt; GSM520671     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520672     1  0.0458      0.981 0.984  0  0 0.016  0  0
#&gt; GSM520673     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520681     1  0.0260      0.992 0.992  0  0 0.008  0  0
#&gt; GSM520682     1  0.0260      0.992 0.992  0  0 0.008  0  0
#&gt; GSM520680     1  0.0260      0.992 0.992  0  0 0.008  0  0
#&gt; GSM520677     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520678     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520679     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520674     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520675     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520676     1  0.0363      0.991 0.988  0  0 0.012  0  0
#&gt; GSM520686     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520687     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520688     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520683     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520684     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520685     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520708     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520706     1  0.0000      0.993 1.000  0  0 0.000  0  0
#&gt; GSM520707     1  0.0000      0.993 1.000  0  0 0.000  0  0
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n cell.type(p) cell.line(p) other(p) k
#> CV:pam 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:pam 51     8.42e-12     1.37e-11 3.29e-04 3
#> CV:pam 51     4.89e-11     2.26e-16 8.17e-08 4
#> CV:pam 51     2.23e-10     3.95e-21 4.56e-12 5
#> CV:pam 51     8.65e-10     7.10e-26 1.98e-15 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.818           0.916       0.951         0.3400 0.887   0.824
#> 4 4 0.785           0.926       0.951         0.4260 0.753   0.537
#> 5 5 0.767           0.891       0.922         0.0314 0.993   0.975
#> 6 6 0.921           0.949       0.968         0.0620 0.958   0.849
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520704     3  0.5733      0.694 0.000 0.324 0.676
#&gt; GSM520705     3  0.5733      0.694 0.000 0.324 0.676
#&gt; GSM520711     3  0.5733      0.694 0.000 0.324 0.676
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM520668     3  0.0424      0.773 0.000 0.008 0.992
#&gt; GSM520669     3  0.0424      0.773 0.000 0.008 0.992
#&gt; GSM520670     3  0.0424      0.773 0.000 0.008 0.992
#&gt; GSM520713     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520714     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520715     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520695     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520696     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520697     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520709     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520710     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520712     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520698     1  0.5216      0.731 0.740 0.000 0.260
#&gt; GSM520699     1  0.5254      0.725 0.736 0.000 0.264
#&gt; GSM520700     1  0.5216      0.731 0.740 0.000 0.260
#&gt; GSM520701     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520702     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520703     1  0.0592      0.957 0.988 0.000 0.012
#&gt; GSM520671     1  0.3879      0.841 0.848 0.000 0.152
#&gt; GSM520672     1  0.5138      0.742 0.748 0.000 0.252
#&gt; GSM520673     1  0.4178      0.822 0.828 0.000 0.172
#&gt; GSM520681     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520708     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.959 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.959 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520704     3  0.0000      0.925 0.000  0 1.000 0.000
#&gt; GSM520705     3  0.0000      0.925 0.000  0 1.000 0.000
#&gt; GSM520711     3  0.0000      0.925 0.000  0 1.000 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM520668     3  0.2704      0.926 0.000  0 0.876 0.124
#&gt; GSM520669     3  0.2704      0.926 0.000  0 0.876 0.124
#&gt; GSM520670     3  0.2704      0.926 0.000  0 0.876 0.124
#&gt; GSM520713     4  0.0000      0.883 0.000  0 0.000 1.000
#&gt; GSM520714     4  0.0000      0.883 0.000  0 0.000 1.000
#&gt; GSM520715     4  0.0000      0.883 0.000  0 0.000 1.000
#&gt; GSM520695     4  0.2814      0.917 0.132  0 0.000 0.868
#&gt; GSM520696     4  0.2814      0.917 0.132  0 0.000 0.868
#&gt; GSM520697     4  0.2760      0.918 0.128  0 0.000 0.872
#&gt; GSM520709     4  0.2973      0.906 0.144  0 0.000 0.856
#&gt; GSM520710     4  0.2921      0.909 0.140  0 0.000 0.860
#&gt; GSM520712     4  0.2647      0.916 0.120  0 0.000 0.880
#&gt; GSM520698     4  0.0376      0.884 0.004  0 0.004 0.992
#&gt; GSM520699     4  0.0376      0.884 0.004  0 0.004 0.992
#&gt; GSM520700     4  0.0376      0.884 0.004  0 0.004 0.992
#&gt; GSM520701     4  0.2814      0.917 0.132  0 0.000 0.868
#&gt; GSM520702     4  0.2704      0.919 0.124  0 0.000 0.876
#&gt; GSM520703     4  0.2589      0.919 0.116  0 0.000 0.884
#&gt; GSM520671     1  0.3837      0.744 0.776  0 0.000 0.224
#&gt; GSM520672     1  0.4564      0.598 0.672  0 0.000 0.328
#&gt; GSM520673     1  0.4040      0.721 0.752  0 0.000 0.248
#&gt; GSM520681     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520682     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520680     1  0.1022      0.934 0.968  0 0.000 0.032
#&gt; GSM520677     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520678     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520679     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520674     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520675     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520676     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520686     1  0.0188      0.950 0.996  0 0.000 0.004
#&gt; GSM520687     1  0.0188      0.950 0.996  0 0.000 0.004
#&gt; GSM520688     1  0.0336      0.948 0.992  0 0.000 0.008
#&gt; GSM520683     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520684     1  0.0921      0.936 0.972  0 0.000 0.028
#&gt; GSM520685     1  0.1474      0.918 0.948  0 0.000 0.052
#&gt; GSM520708     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520706     1  0.0000      0.951 1.000  0 0.000 0.000
#&gt; GSM520707     1  0.0000      0.951 1.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520704     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520705     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520711     5  0.0000      1.000 0.000  0 0.000 0.000  1
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000  0
#&gt; GSM520668     3  0.0000      1.000 0.000  0 1.000 0.000  0
#&gt; GSM520669     3  0.0000      1.000 0.000  0 1.000 0.000  0
#&gt; GSM520670     3  0.0000      1.000 0.000  0 1.000 0.000  0
#&gt; GSM520713     4  0.2818      0.878 0.012  0 0.132 0.856  0
#&gt; GSM520714     4  0.2818      0.878 0.012  0 0.132 0.856  0
#&gt; GSM520715     4  0.2818      0.878 0.012  0 0.132 0.856  0
#&gt; GSM520695     4  0.0880      0.915 0.032  0 0.000 0.968  0
#&gt; GSM520696     4  0.0794      0.915 0.028  0 0.000 0.972  0
#&gt; GSM520697     4  0.0880      0.915 0.032  0 0.000 0.968  0
#&gt; GSM520709     4  0.1043      0.910 0.040  0 0.000 0.960  0
#&gt; GSM520710     4  0.0963      0.911 0.036  0 0.000 0.964  0
#&gt; GSM520712     4  0.0880      0.913 0.032  0 0.000 0.968  0
#&gt; GSM520698     4  0.3039      0.819 0.000  0 0.192 0.808  0
#&gt; GSM520699     4  0.3039      0.819 0.000  0 0.192 0.808  0
#&gt; GSM520700     4  0.3039      0.819 0.000  0 0.192 0.808  0
#&gt; GSM520701     4  0.0963      0.913 0.036  0 0.000 0.964  0
#&gt; GSM520702     4  0.0880      0.915 0.032  0 0.000 0.968  0
#&gt; GSM520703     4  0.0794      0.915 0.028  0 0.000 0.972  0
#&gt; GSM520671     1  0.3796      0.708 0.700  0 0.000 0.300  0
#&gt; GSM520672     1  0.4182      0.552 0.600  0 0.000 0.400  0
#&gt; GSM520673     1  0.3816      0.703 0.696  0 0.000 0.304  0
#&gt; GSM520681     1  0.1792      0.874 0.916  0 0.000 0.084  0
#&gt; GSM520682     1  0.1478      0.875 0.936  0 0.000 0.064  0
#&gt; GSM520680     1  0.2966      0.820 0.816  0 0.000 0.184  0
#&gt; GSM520677     1  0.1270      0.876 0.948  0 0.000 0.052  0
#&gt; GSM520678     1  0.0162      0.860 0.996  0 0.000 0.004  0
#&gt; GSM520679     1  0.0000      0.858 1.000  0 0.000 0.000  0
#&gt; GSM520674     1  0.0162      0.860 0.996  0 0.000 0.004  0
#&gt; GSM520675     1  0.1121      0.875 0.956  0 0.000 0.044  0
#&gt; GSM520676     1  0.0703      0.870 0.976  0 0.000 0.024  0
#&gt; GSM520686     1  0.0794      0.870 0.972  0 0.000 0.028  0
#&gt; GSM520687     1  0.1197      0.876 0.952  0 0.000 0.048  0
#&gt; GSM520688     1  0.1197      0.874 0.952  0 0.000 0.048  0
#&gt; GSM520683     1  0.2280      0.852 0.880  0 0.000 0.120  0
#&gt; GSM520684     1  0.3143      0.815 0.796  0 0.000 0.204  0
#&gt; GSM520685     1  0.4192      0.560 0.596  0 0.000 0.404  0
#&gt; GSM520708     1  0.1792      0.861 0.916  0 0.000 0.084  0
#&gt; GSM520706     1  0.2516      0.843 0.860  0 0.000 0.140  0
#&gt; GSM520707     1  0.2074      0.857 0.896  0 0.000 0.104  0
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM520665     2  0.0146      0.997 0.000 0.996 0.004 0.000  0 0.000
#&gt; GSM520666     2  0.0146      0.997 0.000 0.996 0.004 0.000  0 0.000
#&gt; GSM520667     2  0.0146      0.997 0.000 0.996 0.004 0.000  0 0.000
#&gt; GSM520704     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520705     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520711     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM520692     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520693     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520694     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520689     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520690     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520691     2  0.0000      0.999 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM520668     3  0.0146      1.000 0.000 0.000 0.996 0.000  0 0.004
#&gt; GSM520669     3  0.0146      1.000 0.000 0.000 0.996 0.000  0 0.004
#&gt; GSM520670     3  0.0146      1.000 0.000 0.000 0.996 0.000  0 0.004
#&gt; GSM520713     6  0.0000      0.996 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520714     6  0.0000      0.996 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520715     6  0.0000      0.996 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM520695     4  0.0000      0.996 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520696     4  0.0000      0.996 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520697     4  0.0146      0.997 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520709     4  0.0146      0.997 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520710     4  0.0146      0.997 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520712     4  0.0146      0.997 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520698     6  0.0146      0.996 0.000 0.000 0.000 0.004  0 0.996
#&gt; GSM520699     6  0.0146      0.996 0.000 0.000 0.000 0.004  0 0.996
#&gt; GSM520700     6  0.0146      0.996 0.000 0.000 0.000 0.004  0 0.996
#&gt; GSM520701     4  0.0146      0.997 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM520702     4  0.0000      0.996 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520703     4  0.0000      0.996 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM520671     1  0.2883      0.806 0.788 0.000 0.000 0.212  0 0.000
#&gt; GSM520672     1  0.3756      0.595 0.644 0.000 0.000 0.352  0 0.004
#&gt; GSM520673     1  0.2883      0.806 0.788 0.000 0.000 0.212  0 0.000
#&gt; GSM520681     1  0.0865      0.918 0.964 0.000 0.000 0.036  0 0.000
#&gt; GSM520682     1  0.0790      0.919 0.968 0.000 0.000 0.032  0 0.000
#&gt; GSM520680     1  0.2135      0.870 0.872 0.000 0.000 0.128  0 0.000
#&gt; GSM520677     1  0.0458      0.922 0.984 0.000 0.000 0.016  0 0.000
#&gt; GSM520678     1  0.0000      0.917 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520679     1  0.0000      0.917 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520674     1  0.0000      0.917 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520675     1  0.0547      0.922 0.980 0.000 0.000 0.020  0 0.000
#&gt; GSM520676     1  0.0547      0.922 0.980 0.000 0.000 0.020  0 0.000
#&gt; GSM520686     1  0.0146      0.919 0.996 0.000 0.000 0.004  0 0.000
#&gt; GSM520687     1  0.0363      0.921 0.988 0.000 0.000 0.012  0 0.000
#&gt; GSM520688     1  0.0458      0.922 0.984 0.000 0.000 0.016  0 0.000
#&gt; GSM520683     1  0.0632      0.919 0.976 0.000 0.000 0.024  0 0.000
#&gt; GSM520684     1  0.2092      0.873 0.876 0.000 0.000 0.124  0 0.000
#&gt; GSM520685     1  0.3221      0.743 0.736 0.000 0.000 0.264  0 0.000
#&gt; GSM520708     1  0.0000      0.917 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM520706     1  0.1204      0.905 0.944 0.000 0.000 0.056  0 0.000
#&gt; GSM520707     1  0.0547      0.919 0.980 0.000 0.000 0.020  0 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n cell.type(p) cell.line(p) other(p) k
#> CV:mclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:mclust 51     5.44e-10     1.37e-11 6.36e-06 3
#> CV:mclust 51     2.90e-09     2.26e-16 1.42e-09 4
#> CV:mclust 51     2.23e-10     3.95e-21 4.56e-12 5
#> CV:mclust 51     8.65e-10     2.41e-22 1.91e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.745           0.932       0.911         0.4411 0.788   0.665
#> 4 4 0.793           0.784       0.894         0.2290 0.894   0.761
#> 5 5 0.906           0.903       0.939         0.1153 0.838   0.594
#> 6 6 0.842           0.653       0.856         0.0594 0.989   0.961
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520666     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520667     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520704     2   0.565      0.801 0.000 0.688 0.312
#&gt; GSM520705     2   0.565      0.801 0.000 0.688 0.312
#&gt; GSM520711     2   0.565      0.801 0.000 0.688 0.312
#&gt; GSM520692     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520693     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520694     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520689     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520690     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520691     2   0.000      0.938 0.000 1.000 0.000
#&gt; GSM520668     3   0.562      0.994 0.308 0.000 0.692
#&gt; GSM520669     3   0.562      0.994 0.308 0.000 0.692
#&gt; GSM520670     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520713     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520714     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520715     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520695     1   0.164      0.925 0.956 0.000 0.044
#&gt; GSM520696     1   0.296      0.845 0.900 0.000 0.100
#&gt; GSM520697     1   0.153      0.929 0.960 0.000 0.040
#&gt; GSM520709     1   0.116      0.940 0.972 0.000 0.028
#&gt; GSM520710     1   0.116      0.940 0.972 0.000 0.028
#&gt; GSM520712     1   0.129      0.937 0.968 0.000 0.032
#&gt; GSM520698     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520699     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520700     3   0.565      0.998 0.312 0.000 0.688
#&gt; GSM520701     1   0.153      0.930 0.960 0.000 0.040
#&gt; GSM520702     1   0.465      0.618 0.792 0.000 0.208
#&gt; GSM520703     1   0.525      0.449 0.736 0.000 0.264
#&gt; GSM520671     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520672     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520673     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520681     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520682     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520680     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520677     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520678     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520679     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520674     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520675     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520676     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520686     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520687     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520688     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520683     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520684     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520685     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520708     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520706     1   0.000      0.961 1.000 0.000 0.000
#&gt; GSM520707     1   0.000      0.961 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520704     3  0.3942      1.000 0.000 0.236 0.764 0.000
#&gt; GSM520705     3  0.3942      1.000 0.000 0.236 0.764 0.000
#&gt; GSM520711     3  0.3942      1.000 0.000 0.236 0.764 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520668     4  0.3942      0.660 0.000 0.000 0.236 0.764
#&gt; GSM520669     4  0.3942      0.660 0.000 0.000 0.236 0.764
#&gt; GSM520670     4  0.3942      0.660 0.000 0.000 0.236 0.764
#&gt; GSM520713     4  0.1557      0.737 0.000 0.000 0.056 0.944
#&gt; GSM520714     4  0.1022      0.738 0.000 0.000 0.032 0.968
#&gt; GSM520715     4  0.1302      0.738 0.000 0.000 0.044 0.956
#&gt; GSM520695     1  0.4933      0.337 0.568 0.000 0.000 0.432
#&gt; GSM520696     4  0.4989     -0.110 0.472 0.000 0.000 0.528
#&gt; GSM520697     1  0.4888      0.390 0.588 0.000 0.000 0.412
#&gt; GSM520709     1  0.4830      0.435 0.608 0.000 0.000 0.392
#&gt; GSM520710     1  0.4830      0.435 0.608 0.000 0.000 0.392
#&gt; GSM520712     1  0.4830      0.435 0.608 0.000 0.000 0.392
#&gt; GSM520698     4  0.0921      0.738 0.000 0.000 0.028 0.972
#&gt; GSM520699     4  0.0921      0.738 0.000 0.000 0.028 0.972
#&gt; GSM520700     4  0.2704      0.714 0.000 0.000 0.124 0.876
#&gt; GSM520701     1  0.4877      0.400 0.592 0.000 0.000 0.408
#&gt; GSM520702     4  0.4679      0.313 0.352 0.000 0.000 0.648
#&gt; GSM520703     4  0.4222      0.493 0.272 0.000 0.000 0.728
#&gt; GSM520671     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520677     1  0.1022      0.869 0.968 0.000 0.000 0.032
#&gt; GSM520678     1  0.0817      0.873 0.976 0.000 0.000 0.024
#&gt; GSM520679     1  0.0707      0.875 0.980 0.000 0.000 0.020
#&gt; GSM520674     1  0.0592      0.877 0.984 0.000 0.000 0.016
#&gt; GSM520675     1  0.0188      0.882 0.996 0.000 0.000 0.004
#&gt; GSM520676     1  0.0188      0.882 0.996 0.000 0.000 0.004
#&gt; GSM520686     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.883 1.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.1389      0.854 0.952 0.000 0.000 0.048
#&gt; GSM520706     1  0.0188      0.882 0.996 0.000 0.000 0.004
#&gt; GSM520707     1  0.0000      0.883 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.1732      0.945 0.000 0.920 0.080 0.000 0.000
#&gt; GSM520666     2  0.1732      0.945 0.000 0.920 0.080 0.000 0.000
#&gt; GSM520667     2  0.1732      0.945 0.000 0.920 0.080 0.000 0.000
#&gt; GSM520704     5  0.0703      1.000 0.000 0.024 0.000 0.000 0.976
#&gt; GSM520705     5  0.0703      1.000 0.000 0.024 0.000 0.000 0.976
#&gt; GSM520711     5  0.0703      1.000 0.000 0.024 0.000 0.000 0.976
#&gt; GSM520692     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.973 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.1732      1.000 0.000 0.000 0.920 0.080 0.000
#&gt; GSM520669     3  0.1732      1.000 0.000 0.000 0.920 0.080 0.000
#&gt; GSM520670     3  0.1732      1.000 0.000 0.000 0.920 0.080 0.000
#&gt; GSM520713     4  0.3612      0.594 0.000 0.000 0.268 0.732 0.000
#&gt; GSM520714     4  0.2773      0.724 0.000 0.000 0.164 0.836 0.000
#&gt; GSM520715     4  0.3242      0.668 0.000 0.000 0.216 0.784 0.000
#&gt; GSM520695     4  0.1608      0.844 0.072 0.000 0.000 0.928 0.000
#&gt; GSM520696     4  0.1197      0.846 0.048 0.000 0.000 0.952 0.000
#&gt; GSM520697     4  0.1608      0.844 0.072 0.000 0.000 0.928 0.000
#&gt; GSM520709     4  0.1671      0.842 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520710     4  0.1671      0.842 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520712     4  0.1671      0.842 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520698     4  0.2824      0.759 0.000 0.000 0.116 0.864 0.020
#&gt; GSM520699     4  0.2969      0.750 0.000 0.000 0.128 0.852 0.020
#&gt; GSM520700     4  0.4707      0.236 0.000 0.000 0.392 0.588 0.020
#&gt; GSM520701     4  0.2110      0.844 0.072 0.000 0.000 0.912 0.016
#&gt; GSM520702     4  0.1117      0.835 0.020 0.000 0.000 0.964 0.016
#&gt; GSM520703     4  0.1211      0.837 0.024 0.000 0.000 0.960 0.016
#&gt; GSM520671     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.0162      0.971 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520682     1  0.0162      0.971 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520680     1  0.0162      0.971 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520677     1  0.1952      0.919 0.912 0.000 0.000 0.084 0.004
#&gt; GSM520678     1  0.1571      0.941 0.936 0.000 0.000 0.060 0.004
#&gt; GSM520679     1  0.1571      0.941 0.936 0.000 0.000 0.060 0.004
#&gt; GSM520674     1  0.1430      0.947 0.944 0.000 0.000 0.052 0.004
#&gt; GSM520675     1  0.1124      0.957 0.960 0.000 0.000 0.036 0.004
#&gt; GSM520676     1  0.1041      0.958 0.964 0.000 0.000 0.032 0.004
#&gt; GSM520686     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.972 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.2605      0.818 0.852 0.000 0.000 0.148 0.000
#&gt; GSM520706     1  0.0404      0.968 0.988 0.000 0.000 0.012 0.000
#&gt; GSM520707     1  0.0162      0.971 0.996 0.000 0.000 0.004 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.3309      0.790 0.000 0.720 0.000 0.000 0.000 0.280
#&gt; GSM520666     2  0.3309      0.790 0.000 0.720 0.000 0.000 0.000 0.280
#&gt; GSM520667     2  0.3309      0.790 0.000 0.720 0.000 0.000 0.000 0.280
#&gt; GSM520704     5  0.0260      1.000 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM520705     5  0.0260      1.000 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM520711     5  0.0260      1.000 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; GSM520692     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.901 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0146      0.901 0.000 0.996 0.004 0.000 0.000 0.000
#&gt; GSM520690     2  0.0146      0.901 0.000 0.996 0.004 0.000 0.000 0.000
#&gt; GSM520691     2  0.0291      0.899 0.000 0.992 0.004 0.000 0.000 0.004
#&gt; GSM520668     3  0.0146      1.000 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM520669     3  0.0146      1.000 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM520670     3  0.0146      1.000 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM520713     6  0.6053      0.000 0.000 0.000 0.256 0.368 0.000 0.376
#&gt; GSM520714     4  0.5943     -0.870 0.000 0.000 0.216 0.404 0.000 0.380
#&gt; GSM520715     4  0.6005     -0.936 0.000 0.000 0.236 0.384 0.000 0.380
#&gt; GSM520695     4  0.3717      0.182 0.000 0.000 0.000 0.616 0.000 0.384
#&gt; GSM520696     4  0.3717      0.182 0.000 0.000 0.000 0.616 0.000 0.384
#&gt; GSM520697     4  0.3706      0.182 0.000 0.000 0.000 0.620 0.000 0.380
#&gt; GSM520709     4  0.3717      0.184 0.000 0.000 0.000 0.616 0.000 0.384
#&gt; GSM520710     4  0.3717      0.184 0.000 0.000 0.000 0.616 0.000 0.384
#&gt; GSM520712     4  0.3727      0.176 0.000 0.000 0.000 0.612 0.000 0.388
#&gt; GSM520698     4  0.2265      0.382 0.000 0.000 0.052 0.896 0.000 0.052
#&gt; GSM520699     4  0.2201      0.385 0.000 0.000 0.048 0.900 0.000 0.052
#&gt; GSM520700     4  0.4007      0.231 0.000 0.000 0.220 0.728 0.000 0.052
#&gt; GSM520701     4  0.0363      0.418 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; GSM520702     4  0.0146      0.418 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM520703     4  0.0458      0.417 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM520671     1  0.0790      0.893 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM520672     1  0.0458      0.895 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM520673     1  0.1327      0.885 0.936 0.000 0.000 0.000 0.000 0.064
#&gt; GSM520681     1  0.0363      0.896 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM520682     1  0.0692      0.895 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM520680     1  0.2948      0.832 0.804 0.000 0.000 0.000 0.008 0.188
#&gt; GSM520677     1  0.4322      0.747 0.672 0.000 0.000 0.032 0.008 0.288
#&gt; GSM520678     1  0.4001      0.778 0.704 0.000 0.000 0.020 0.008 0.268
#&gt; GSM520679     1  0.4152      0.771 0.696 0.000 0.000 0.028 0.008 0.268
#&gt; GSM520674     1  0.3809      0.788 0.716 0.000 0.000 0.012 0.008 0.264
#&gt; GSM520675     1  0.3787      0.790 0.720 0.000 0.000 0.012 0.008 0.260
#&gt; GSM520676     1  0.3809      0.788 0.716 0.000 0.000 0.012 0.008 0.264
#&gt; GSM520686     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.1225      0.864 0.952 0.000 0.000 0.036 0.000 0.012
#&gt; GSM520706     1  0.0146      0.894 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM520707     1  0.0000      0.896 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n cell.type(p) cell.line(p) other(p) k
#> CV:NMF 51     1.46e-11     9.31e-07 5.89e-03 2
#> CV:NMF 50     1.39e-11     8.89e-10 2.44e-04 3
#> CV:NMF 42     4.01e-09     8.59e-13 1.50e-05 4
#> CV:NMF 50     3.61e-10     1.86e-20 7.55e-12 5
#> CV:NMF 36     7.49e-08     1.21e-11 7.41e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.713           0.817       0.813         0.6004 0.718   0.554
#> 4 4 0.649           0.741       0.770         0.0669 0.979   0.940
#> 5 5 0.960           0.976       0.987         0.1942 0.915   0.743
#> 6 6 1.000           0.976       0.987         0.0229 0.993   0.971
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520704     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520705     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520711     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520668     3   0.000      0.409 0.000  0 1.000
#&gt; GSM520669     3   0.000      0.409 0.000  0 1.000
#&gt; GSM520670     3   0.000      0.409 0.000  0 1.000
#&gt; GSM520713     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520714     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520715     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520695     1   0.116      0.952 0.972  0 0.028
#&gt; GSM520696     1   0.116      0.952 0.972  0 0.028
#&gt; GSM520697     1   0.116      0.952 0.972  0 0.028
#&gt; GSM520709     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520710     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520712     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520698     3   0.418      0.274 0.172  0 0.828
#&gt; GSM520699     3   0.418      0.274 0.172  0 0.828
#&gt; GSM520700     3   0.418      0.274 0.172  0 0.828
#&gt; GSM520701     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520702     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520703     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520671     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520672     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520673     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520681     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520682     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520680     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520677     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520678     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520679     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520674     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520675     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520676     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520686     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520687     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520688     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520683     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520684     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520685     3   0.631      0.718 0.496  0 0.504
#&gt; GSM520708     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520706     1   0.000      0.989 1.000  0 0.000
#&gt; GSM520707     1   0.000      0.989 1.000  0 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520666     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520667     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520704     3   0.468     1.0000 0.000 0.352 0.648 0.000
#&gt; GSM520705     3   0.468     1.0000 0.000 0.352 0.648 0.000
#&gt; GSM520711     3   0.468     1.0000 0.000 0.352 0.648 0.000
#&gt; GSM520692     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520693     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520694     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520689     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520690     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520691     2   0.000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM520668     1   0.468    -0.1123 0.648 0.000 0.352 0.000
#&gt; GSM520669     1   0.468    -0.1123 0.648 0.000 0.352 0.000
#&gt; GSM520670     1   0.468    -0.1123 0.648 0.000 0.352 0.000
#&gt; GSM520713     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520714     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520715     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520695     4   0.104     0.9609 0.020 0.000 0.008 0.972
#&gt; GSM520696     4   0.104     0.9609 0.020 0.000 0.008 0.972
#&gt; GSM520697     4   0.104     0.9609 0.020 0.000 0.008 0.972
#&gt; GSM520709     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520710     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520712     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520698     1   0.632    -0.0249 0.660 0.000 0.168 0.172
#&gt; GSM520699     1   0.632    -0.0249 0.660 0.000 0.168 0.172
#&gt; GSM520700     1   0.632    -0.0249 0.660 0.000 0.168 0.172
#&gt; GSM520701     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520702     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520703     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520671     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520672     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520673     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520681     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520682     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520680     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520677     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520678     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520679     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520674     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520675     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520676     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520686     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520687     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520688     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520683     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520684     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520685     1   0.499     0.6358 0.528 0.000 0.000 0.472
#&gt; GSM520708     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520706     4   0.000     0.9905 0.000 0.000 0.000 1.000
#&gt; GSM520707     4   0.000     0.9905 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette   p1 p2    p3    p4 p5
#&gt; GSM520665     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520666     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520667     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520704     5  0.0000      1.000 0.00  0 0.000 0.000  1
#&gt; GSM520705     5  0.0000      1.000 0.00  0 0.000 0.000  1
#&gt; GSM520711     5  0.0000      1.000 0.00  0 0.000 0.000  1
#&gt; GSM520692     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520693     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520694     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520689     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520690     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520691     2  0.0000      1.000 0.00  1 0.000 0.000  0
#&gt; GSM520668     3  0.0000      0.823 0.00  0 1.000 0.000  0
#&gt; GSM520669     3  0.0000      0.823 0.00  0 1.000 0.000  0
#&gt; GSM520670     3  0.0000      0.823 0.00  0 1.000 0.000  0
#&gt; GSM520713     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520714     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520715     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520695     4  0.0898      0.968 0.02  0 0.008 0.972  0
#&gt; GSM520696     4  0.0898      0.968 0.02  0 0.008 0.972  0
#&gt; GSM520697     4  0.0898      0.968 0.02  0 0.008 0.972  0
#&gt; GSM520709     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520710     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520712     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520698     3  0.3438      0.828 0.02  0 0.808 0.172  0
#&gt; GSM520699     3  0.3438      0.828 0.02  0 0.808 0.172  0
#&gt; GSM520700     3  0.3438      0.828 0.02  0 0.808 0.172  0
#&gt; GSM520701     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520702     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520703     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520671     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520672     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520673     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520681     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520682     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520680     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520677     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520678     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520679     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520674     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520675     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520676     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520686     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520687     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520688     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520683     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520684     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520685     1  0.0000      1.000 1.00  0 0.000 0.000  0
#&gt; GSM520708     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520706     4  0.0000      0.992 0.00  0 0.000 1.000  0
#&gt; GSM520707     4  0.0000      0.992 0.00  0 0.000 1.000  0
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1    p2    p3  p4 p5    p6
#&gt; GSM520665     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520666     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520667     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520704     5  0.0000      1.000  0 0.000 0.000 0.0  1 0.000
#&gt; GSM520705     5  0.0000      1.000  0 0.000 0.000 0.0  1 0.000
#&gt; GSM520711     5  0.0000      1.000  0 0.000 0.000 0.0  1 0.000
#&gt; GSM520692     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520693     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520694     2  0.0000      0.996  0 1.000 0.000 0.0  0 0.000
#&gt; GSM520689     2  0.0363      0.992  0 0.988 0.000 0.0  0 0.012
#&gt; GSM520690     2  0.0363      0.992  0 0.988 0.000 0.0  0 0.012
#&gt; GSM520691     2  0.0363      0.992  0 0.988 0.000 0.0  0 0.012
#&gt; GSM520668     3  0.0000      1.000  0 0.000 1.000 0.0  0 0.000
#&gt; GSM520669     3  0.0000      1.000  0 0.000 1.000 0.0  0 0.000
#&gt; GSM520670     3  0.0000      1.000  0 0.000 1.000 0.0  0 0.000
#&gt; GSM520713     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520714     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520715     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520695     4  0.2793      0.786  0 0.000 0.000 0.8  0 0.200
#&gt; GSM520696     4  0.2793      0.786  0 0.000 0.000 0.8  0 0.200
#&gt; GSM520697     4  0.2793      0.786  0 0.000 0.000 0.8  0 0.200
#&gt; GSM520709     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520710     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520712     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520698     6  0.0363      1.000  0 0.000 0.012 0.0  0 0.988
#&gt; GSM520699     6  0.0363      1.000  0 0.000 0.012 0.0  0 0.988
#&gt; GSM520700     6  0.0363      1.000  0 0.000 0.012 0.0  0 0.988
#&gt; GSM520701     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520702     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520703     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520671     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520672     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520673     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520681     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520682     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520680     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520677     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520678     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520679     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520674     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520675     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520676     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520686     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520687     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520688     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520683     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520684     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520685     1  0.0000      1.000  1 0.000 0.000 0.0  0 0.000
#&gt; GSM520708     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520706     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
#&gt; GSM520707     4  0.0000      0.955  0 0.000 0.000 1.0  0 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> MAD:hclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> MAD:hclust 45     1.69e-10     8.33e-09 7.28e-07 3
#> MAD:hclust 45     9.25e-10     6.46e-13 6.72e-08 4
#> MAD:hclust 51     2.23e-10     1.24e-16 5.19e-11 5
#> MAD:hclust 51     8.65e-10     2.54e-19 7.24e-14 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.960       0.963         0.3821 0.633   0.633
#> 3 3 0.645           0.920       0.898         0.6094 0.704   0.532
#> 4 4 0.859           0.890       0.895         0.1425 0.942   0.829
#> 5 5 0.704           0.771       0.825         0.0727 1.000   1.000
#> 6 6 0.740           0.661       0.755         0.0536 0.976   0.917
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM520665     2  0.0376      0.987 0.004 0.996
#&gt; GSM520666     2  0.0376      0.987 0.004 0.996
#&gt; GSM520667     2  0.0376      0.987 0.004 0.996
#&gt; GSM520704     2  0.2236      0.966 0.036 0.964
#&gt; GSM520705     2  0.2236      0.966 0.036 0.964
#&gt; GSM520711     2  0.2236      0.966 0.036 0.964
#&gt; GSM520692     2  0.0000      0.987 0.000 1.000
#&gt; GSM520693     2  0.0000      0.987 0.000 1.000
#&gt; GSM520694     2  0.0000      0.987 0.000 1.000
#&gt; GSM520689     2  0.0376      0.987 0.004 0.996
#&gt; GSM520690     2  0.0376      0.987 0.004 0.996
#&gt; GSM520691     2  0.0376      0.987 0.004 0.996
#&gt; GSM520668     1  0.5737      0.899 0.864 0.136
#&gt; GSM520669     1  0.5737      0.899 0.864 0.136
#&gt; GSM520670     1  0.5737      0.899 0.864 0.136
#&gt; GSM520713     1  0.2603      0.955 0.956 0.044
#&gt; GSM520714     1  0.2603      0.955 0.956 0.044
#&gt; GSM520715     1  0.2603      0.955 0.956 0.044
#&gt; GSM520695     1  0.2236      0.957 0.964 0.036
#&gt; GSM520696     1  0.2236      0.957 0.964 0.036
#&gt; GSM520697     1  0.2236      0.957 0.964 0.036
#&gt; GSM520709     1  0.2603      0.955 0.956 0.044
#&gt; GSM520710     1  0.2603      0.955 0.956 0.044
#&gt; GSM520712     1  0.2603      0.955 0.956 0.044
#&gt; GSM520698     1  0.2603      0.955 0.956 0.044
#&gt; GSM520699     1  0.2603      0.955 0.956 0.044
#&gt; GSM520700     1  0.2423      0.959 0.960 0.040
#&gt; GSM520701     1  0.2603      0.955 0.956 0.044
#&gt; GSM520702     1  0.2603      0.955 0.956 0.044
#&gt; GSM520703     1  0.2603      0.955 0.956 0.044
#&gt; GSM520671     1  0.2236      0.960 0.964 0.036
#&gt; GSM520672     1  0.2236      0.960 0.964 0.036
#&gt; GSM520673     1  0.2236      0.960 0.964 0.036
#&gt; GSM520681     1  0.2043      0.961 0.968 0.032
#&gt; GSM520682     1  0.2043      0.961 0.968 0.032
#&gt; GSM520680     1  0.2236      0.960 0.964 0.036
#&gt; GSM520677     1  0.2236      0.960 0.964 0.036
#&gt; GSM520678     1  0.2236      0.960 0.964 0.036
#&gt; GSM520679     1  0.2236      0.960 0.964 0.036
#&gt; GSM520674     1  0.2236      0.960 0.964 0.036
#&gt; GSM520675     1  0.2043      0.961 0.968 0.032
#&gt; GSM520676     1  0.2236      0.960 0.964 0.036
#&gt; GSM520686     1  0.2236      0.960 0.964 0.036
#&gt; GSM520687     1  0.2236      0.960 0.964 0.036
#&gt; GSM520688     1  0.2236      0.960 0.964 0.036
#&gt; GSM520683     1  0.2043      0.961 0.968 0.032
#&gt; GSM520684     1  0.2236      0.960 0.964 0.036
#&gt; GSM520685     1  0.2236      0.960 0.964 0.036
#&gt; GSM520708     1  0.1633      0.957 0.976 0.024
#&gt; GSM520706     1  0.1633      0.957 0.976 0.024
#&gt; GSM520707     1  0.1633      0.957 0.976 0.024
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.1015      0.976 0.012 0.980 0.008
#&gt; GSM520666     2  0.1015      0.976 0.012 0.980 0.008
#&gt; GSM520667     2  0.1015      0.976 0.012 0.980 0.008
#&gt; GSM520704     2  0.3295      0.947 0.096 0.896 0.008
#&gt; GSM520705     2  0.3295      0.947 0.096 0.896 0.008
#&gt; GSM520711     2  0.3295      0.947 0.096 0.896 0.008
#&gt; GSM520692     2  0.0424      0.977 0.000 0.992 0.008
#&gt; GSM520693     2  0.0424      0.977 0.000 0.992 0.008
#&gt; GSM520694     2  0.0424      0.977 0.000 0.992 0.008
#&gt; GSM520689     2  0.1315      0.975 0.020 0.972 0.008
#&gt; GSM520690     2  0.1315      0.975 0.020 0.972 0.008
#&gt; GSM520691     2  0.1315      0.975 0.020 0.972 0.008
#&gt; GSM520668     1  0.5803      0.800 0.760 0.028 0.212
#&gt; GSM520669     1  0.5803      0.800 0.760 0.028 0.212
#&gt; GSM520670     1  0.5803      0.800 0.760 0.028 0.212
#&gt; GSM520713     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520714     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520715     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520695     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520696     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520697     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520709     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520710     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520712     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520698     3  0.2066      0.887 0.060 0.000 0.940
#&gt; GSM520699     3  0.2066      0.887 0.060 0.000 0.940
#&gt; GSM520700     3  0.6308     -0.172 0.492 0.000 0.508
#&gt; GSM520701     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520702     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520703     3  0.0000      0.942 0.000 0.000 1.000
#&gt; GSM520671     1  0.4784      0.957 0.796 0.004 0.200
#&gt; GSM520672     1  0.4784      0.957 0.796 0.004 0.200
#&gt; GSM520673     1  0.4784      0.957 0.796 0.004 0.200
#&gt; GSM520681     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520682     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520680     1  0.4784      0.957 0.796 0.004 0.200
#&gt; GSM520677     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520678     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520679     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520674     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520675     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520676     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520686     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520687     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520688     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520683     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520684     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520685     1  0.4702      0.965 0.788 0.000 0.212
#&gt; GSM520708     3  0.2165      0.886 0.064 0.000 0.936
#&gt; GSM520706     3  0.2165      0.886 0.064 0.000 0.936
#&gt; GSM520707     3  0.2165      0.886 0.064 0.000 0.936
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.1913      0.921 0.000 0.940 0.040 0.020
#&gt; GSM520666     2  0.1913      0.921 0.000 0.940 0.040 0.020
#&gt; GSM520667     2  0.1913      0.921 0.000 0.940 0.040 0.020
#&gt; GSM520704     2  0.4446      0.829 0.000 0.776 0.196 0.028
#&gt; GSM520705     2  0.4446      0.829 0.000 0.776 0.196 0.028
#&gt; GSM520711     2  0.4446      0.829 0.000 0.776 0.196 0.028
#&gt; GSM520692     2  0.0336      0.928 0.000 0.992 0.000 0.008
#&gt; GSM520693     2  0.0336      0.928 0.000 0.992 0.000 0.008
#&gt; GSM520694     2  0.0336      0.928 0.000 0.992 0.000 0.008
#&gt; GSM520689     2  0.1004      0.927 0.000 0.972 0.024 0.004
#&gt; GSM520690     2  0.1004      0.927 0.000 0.972 0.024 0.004
#&gt; GSM520691     2  0.1004      0.927 0.000 0.972 0.024 0.004
#&gt; GSM520668     3  0.5396      0.881 0.240 0.016 0.716 0.028
#&gt; GSM520669     3  0.5396      0.881 0.240 0.016 0.716 0.028
#&gt; GSM520670     3  0.5396      0.881 0.240 0.016 0.716 0.028
#&gt; GSM520713     4  0.2124      0.915 0.068 0.000 0.008 0.924
#&gt; GSM520714     4  0.2124      0.915 0.068 0.000 0.008 0.924
#&gt; GSM520715     4  0.2124      0.915 0.068 0.000 0.008 0.924
#&gt; GSM520695     4  0.2198      0.914 0.072 0.000 0.008 0.920
#&gt; GSM520696     4  0.2198      0.914 0.072 0.000 0.008 0.920
#&gt; GSM520697     4  0.2198      0.914 0.072 0.000 0.008 0.920
#&gt; GSM520709     4  0.1792      0.915 0.068 0.000 0.000 0.932
#&gt; GSM520710     4  0.1792      0.915 0.068 0.000 0.000 0.932
#&gt; GSM520712     4  0.1792      0.915 0.068 0.000 0.000 0.932
#&gt; GSM520698     4  0.5713      0.438 0.040 0.000 0.340 0.620
#&gt; GSM520699     4  0.5713      0.438 0.040 0.000 0.340 0.620
#&gt; GSM520700     3  0.6796      0.598 0.152 0.000 0.596 0.252
#&gt; GSM520701     4  0.2489      0.913 0.068 0.000 0.020 0.912
#&gt; GSM520702     4  0.2489      0.913 0.068 0.000 0.020 0.912
#&gt; GSM520703     4  0.2489      0.913 0.068 0.000 0.020 0.912
#&gt; GSM520671     1  0.2216      0.934 0.908 0.000 0.092 0.000
#&gt; GSM520672     1  0.2216      0.934 0.908 0.000 0.092 0.000
#&gt; GSM520673     1  0.2216      0.934 0.908 0.000 0.092 0.000
#&gt; GSM520681     1  0.0000      0.943 1.000 0.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.943 1.000 0.000 0.000 0.000
#&gt; GSM520680     1  0.2216      0.934 0.908 0.000 0.092 0.000
#&gt; GSM520677     1  0.1211      0.947 0.960 0.000 0.040 0.000
#&gt; GSM520678     1  0.1211      0.947 0.960 0.000 0.040 0.000
#&gt; GSM520679     1  0.1211      0.947 0.960 0.000 0.040 0.000
#&gt; GSM520674     1  0.1211      0.947 0.960 0.000 0.040 0.000
#&gt; GSM520675     1  0.0000      0.943 1.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.1211      0.947 0.960 0.000 0.040 0.000
#&gt; GSM520686     1  0.1389      0.941 0.952 0.000 0.048 0.000
#&gt; GSM520687     1  0.1389      0.941 0.952 0.000 0.048 0.000
#&gt; GSM520688     1  0.1389      0.941 0.952 0.000 0.048 0.000
#&gt; GSM520683     1  0.0000      0.943 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.1389      0.941 0.952 0.000 0.048 0.000
#&gt; GSM520685     1  0.0188      0.944 0.996 0.000 0.004 0.000
#&gt; GSM520708     4  0.4100      0.839 0.148 0.000 0.036 0.816
#&gt; GSM520706     4  0.4100      0.839 0.148 0.000 0.036 0.816
#&gt; GSM520707     4  0.4100      0.839 0.148 0.000 0.036 0.816
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2  0.2234      0.862 0.000 0.916 0.036 0.004 NA
#&gt; GSM520666     2  0.2278      0.862 0.000 0.916 0.032 0.008 NA
#&gt; GSM520667     2  0.2234      0.862 0.000 0.916 0.036 0.004 NA
#&gt; GSM520704     2  0.4341      0.706 0.000 0.628 0.008 0.000 NA
#&gt; GSM520705     2  0.4341      0.706 0.000 0.628 0.008 0.000 NA
#&gt; GSM520711     2  0.4721      0.706 0.000 0.628 0.020 0.004 NA
#&gt; GSM520692     2  0.0727      0.869 0.000 0.980 0.004 0.004 NA
#&gt; GSM520693     2  0.0727      0.869 0.000 0.980 0.004 0.004 NA
#&gt; GSM520694     2  0.0727      0.869 0.000 0.980 0.004 0.004 NA
#&gt; GSM520689     2  0.2308      0.855 0.000 0.912 0.048 0.004 NA
#&gt; GSM520690     2  0.2308      0.855 0.000 0.912 0.048 0.004 NA
#&gt; GSM520691     2  0.2359      0.855 0.000 0.912 0.044 0.008 NA
#&gt; GSM520668     3  0.3246      0.888 0.120 0.008 0.848 0.024 NA
#&gt; GSM520669     3  0.3246      0.888 0.120 0.008 0.848 0.024 NA
#&gt; GSM520670     3  0.3246      0.888 0.120 0.008 0.848 0.024 NA
#&gt; GSM520713     4  0.1300      0.806 0.028 0.000 0.000 0.956 NA
#&gt; GSM520714     4  0.1300      0.806 0.028 0.000 0.000 0.956 NA
#&gt; GSM520715     4  0.1300      0.806 0.028 0.000 0.000 0.956 NA
#&gt; GSM520695     4  0.3653      0.752 0.036 0.000 0.012 0.828 NA
#&gt; GSM520696     4  0.3653      0.752 0.036 0.000 0.012 0.828 NA
#&gt; GSM520697     4  0.3653      0.752 0.036 0.000 0.012 0.828 NA
#&gt; GSM520709     4  0.1493      0.807 0.028 0.000 0.000 0.948 NA
#&gt; GSM520710     4  0.1493      0.807 0.028 0.000 0.000 0.948 NA
#&gt; GSM520712     4  0.0794      0.807 0.028 0.000 0.000 0.972 NA
#&gt; GSM520698     4  0.6737      0.205 0.024 0.000 0.340 0.492 NA
#&gt; GSM520699     4  0.6737      0.205 0.024 0.000 0.340 0.492 NA
#&gt; GSM520700     3  0.6678      0.578 0.072 0.000 0.608 0.184 NA
#&gt; GSM520701     4  0.2921      0.799 0.028 0.000 0.020 0.884 NA
#&gt; GSM520702     4  0.2921      0.799 0.028 0.000 0.020 0.884 NA
#&gt; GSM520703     4  0.2921      0.799 0.028 0.000 0.020 0.884 NA
#&gt; GSM520671     1  0.4609      0.808 0.744 0.000 0.104 0.000 NA
#&gt; GSM520672     1  0.4609      0.808 0.744 0.000 0.104 0.000 NA
#&gt; GSM520673     1  0.4609      0.808 0.744 0.000 0.104 0.000 NA
#&gt; GSM520681     1  0.1571      0.810 0.936 0.000 0.004 0.000 NA
#&gt; GSM520682     1  0.1571      0.810 0.936 0.000 0.004 0.000 NA
#&gt; GSM520680     1  0.4609      0.808 0.744 0.000 0.104 0.000 NA
#&gt; GSM520677     1  0.2729      0.807 0.884 0.000 0.060 0.000 NA
#&gt; GSM520678     1  0.2729      0.807 0.884 0.000 0.060 0.000 NA
#&gt; GSM520679     1  0.2729      0.807 0.884 0.000 0.060 0.000 NA
#&gt; GSM520674     1  0.2729      0.807 0.884 0.000 0.060 0.000 NA
#&gt; GSM520675     1  0.1121      0.812 0.956 0.000 0.000 0.000 NA
#&gt; GSM520676     1  0.2729      0.807 0.884 0.000 0.060 0.000 NA
#&gt; GSM520686     1  0.3958      0.809 0.780 0.000 0.044 0.000 NA
#&gt; GSM520687     1  0.3958      0.809 0.780 0.000 0.044 0.000 NA
#&gt; GSM520688     1  0.3958      0.809 0.780 0.000 0.044 0.000 NA
#&gt; GSM520683     1  0.1638      0.811 0.932 0.000 0.004 0.000 NA
#&gt; GSM520684     1  0.3995      0.808 0.776 0.000 0.044 0.000 NA
#&gt; GSM520685     1  0.3209      0.817 0.812 0.000 0.008 0.000 NA
#&gt; GSM520708     4  0.6459      0.573 0.184 0.000 0.032 0.600 NA
#&gt; GSM520706     4  0.6459      0.573 0.184 0.000 0.032 0.600 NA
#&gt; GSM520707     4  0.6459      0.573 0.184 0.000 0.032 0.600 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM520665     2  0.2758      0.843 0.000 0.872 0.012 0.000 0.036 NA
#&gt; GSM520666     2  0.2758      0.843 0.000 0.872 0.012 0.000 0.036 NA
#&gt; GSM520667     2  0.2758      0.843 0.000 0.872 0.012 0.000 0.036 NA
#&gt; GSM520704     2  0.4870      0.688 0.000 0.612 0.008 0.000 0.320 NA
#&gt; GSM520705     2  0.4870      0.688 0.000 0.612 0.008 0.000 0.320 NA
#&gt; GSM520711     2  0.5137      0.688 0.000 0.612 0.012 0.004 0.304 NA
#&gt; GSM520692     2  0.0146      0.860 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520693     2  0.0146      0.860 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520694     2  0.0146      0.860 0.000 0.996 0.000 0.000 0.004 NA
#&gt; GSM520689     2  0.1901      0.852 0.000 0.924 0.028 0.000 0.008 NA
#&gt; GSM520690     2  0.1901      0.852 0.000 0.924 0.028 0.000 0.008 NA
#&gt; GSM520691     2  0.1901      0.852 0.000 0.924 0.028 0.000 0.008 NA
#&gt; GSM520668     3  0.1732      0.783 0.072 0.000 0.920 0.004 0.000 NA
#&gt; GSM520669     3  0.1588      0.784 0.072 0.000 0.924 0.004 0.000 NA
#&gt; GSM520670     3  0.1588      0.784 0.072 0.000 0.924 0.004 0.000 NA
#&gt; GSM520713     4  0.2321      0.605 0.008 0.000 0.004 0.904 0.052 NA
#&gt; GSM520714     4  0.2321      0.605 0.008 0.000 0.004 0.904 0.052 NA
#&gt; GSM520715     4  0.2321      0.605 0.008 0.000 0.004 0.904 0.052 NA
#&gt; GSM520695     4  0.4626      0.165 0.008 0.000 0.016 0.680 0.264 NA
#&gt; GSM520696     4  0.4626      0.165 0.008 0.000 0.016 0.680 0.264 NA
#&gt; GSM520697     4  0.4626      0.165 0.008 0.000 0.016 0.680 0.264 NA
#&gt; GSM520709     4  0.1149      0.628 0.008 0.000 0.000 0.960 0.008 NA
#&gt; GSM520710     4  0.1149      0.628 0.008 0.000 0.000 0.960 0.008 NA
#&gt; GSM520712     4  0.0717      0.619 0.008 0.000 0.000 0.976 0.016 NA
#&gt; GSM520698     5  0.6324      1.000 0.004 0.000 0.276 0.332 0.384 NA
#&gt; GSM520699     5  0.6324      1.000 0.004 0.000 0.276 0.332 0.384 NA
#&gt; GSM520700     3  0.5929     -0.322 0.020 0.000 0.492 0.112 0.372 NA
#&gt; GSM520701     4  0.3090      0.591 0.008 0.000 0.016 0.864 0.060 NA
#&gt; GSM520702     4  0.3090      0.591 0.008 0.000 0.016 0.864 0.060 NA
#&gt; GSM520703     4  0.3090      0.591 0.008 0.000 0.016 0.864 0.060 NA
#&gt; GSM520671     1  0.1644      0.729 0.932 0.000 0.000 0.000 0.040 NA
#&gt; GSM520672     1  0.1644      0.729 0.932 0.000 0.000 0.000 0.040 NA
#&gt; GSM520673     1  0.1644      0.729 0.932 0.000 0.000 0.000 0.040 NA
#&gt; GSM520681     1  0.3833      0.717 0.556 0.000 0.000 0.000 0.000 NA
#&gt; GSM520682     1  0.3833      0.717 0.556 0.000 0.000 0.000 0.000 NA
#&gt; GSM520680     1  0.1644      0.729 0.932 0.000 0.000 0.000 0.040 NA
#&gt; GSM520677     1  0.3933      0.735 0.740 0.000 0.004 0.000 0.040 NA
#&gt; GSM520678     1  0.3933      0.735 0.740 0.000 0.004 0.000 0.040 NA
#&gt; GSM520679     1  0.3933      0.735 0.740 0.000 0.004 0.000 0.040 NA
#&gt; GSM520674     1  0.3933      0.735 0.740 0.000 0.004 0.000 0.040 NA
#&gt; GSM520675     1  0.4568      0.741 0.612 0.000 0.004 0.000 0.040 NA
#&gt; GSM520676     1  0.3933      0.735 0.740 0.000 0.004 0.000 0.040 NA
#&gt; GSM520686     1  0.3361      0.741 0.788 0.000 0.004 0.000 0.020 NA
#&gt; GSM520687     1  0.3361      0.741 0.788 0.000 0.004 0.000 0.020 NA
#&gt; GSM520688     1  0.3361      0.741 0.788 0.000 0.004 0.000 0.020 NA
#&gt; GSM520683     1  0.3833      0.717 0.556 0.000 0.000 0.000 0.000 NA
#&gt; GSM520684     1  0.3398      0.735 0.768 0.000 0.004 0.000 0.012 NA
#&gt; GSM520685     1  0.3426      0.736 0.764 0.000 0.004 0.000 0.012 NA
#&gt; GSM520708     4  0.5699      0.266 0.012 0.000 0.004 0.448 0.096 NA
#&gt; GSM520706     4  0.5699      0.266 0.012 0.000 0.004 0.448 0.096 NA
#&gt; GSM520707     4  0.5699      0.266 0.012 0.000 0.004 0.448 0.096 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> MAD:kmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> MAD:kmeans 50     1.39e-11     6.67e-10 1.12e-06 3
#> MAD:kmeans 49     1.30e-10     5.62e-12 7.82e-10 4
#> MAD:kmeans 49     1.30e-10     5.62e-12 7.82e-10 5
#> MAD:kmeans 44     6.42e-09     1.74e-11 5.32e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.988       0.994         0.4285 0.576   0.576
#> 3 3 1.000           0.994       0.997         0.5819 0.746   0.559
#> 4 4 1.000           0.993       0.994         0.0841 0.929   0.786
#> 5 5 0.844           0.826       0.868         0.0589 0.972   0.894
#> 6 6 0.818           0.699       0.789         0.0466 0.937   0.736
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM520665     2   0.000      1.000 0.000 1.000
#&gt; GSM520666     2   0.000      1.000 0.000 1.000
#&gt; GSM520667     2   0.000      1.000 0.000 1.000
#&gt; GSM520704     2   0.000      1.000 0.000 1.000
#&gt; GSM520705     2   0.000      1.000 0.000 1.000
#&gt; GSM520711     2   0.000      1.000 0.000 1.000
#&gt; GSM520692     2   0.000      1.000 0.000 1.000
#&gt; GSM520693     2   0.000      1.000 0.000 1.000
#&gt; GSM520694     2   0.000      1.000 0.000 1.000
#&gt; GSM520689     2   0.000      1.000 0.000 1.000
#&gt; GSM520690     2   0.000      1.000 0.000 1.000
#&gt; GSM520691     2   0.000      1.000 0.000 1.000
#&gt; GSM520668     2   0.000      1.000 0.000 1.000
#&gt; GSM520669     2   0.000      1.000 0.000 1.000
#&gt; GSM520670     2   0.000      1.000 0.000 1.000
#&gt; GSM520713     1   0.000      0.991 1.000 0.000
#&gt; GSM520714     1   0.000      0.991 1.000 0.000
#&gt; GSM520715     1   0.000      0.991 1.000 0.000
#&gt; GSM520695     1   0.000      0.991 1.000 0.000
#&gt; GSM520696     1   0.000      0.991 1.000 0.000
#&gt; GSM520697     1   0.000      0.991 1.000 0.000
#&gt; GSM520709     1   0.000      0.991 1.000 0.000
#&gt; GSM520710     1   0.000      0.991 1.000 0.000
#&gt; GSM520712     1   0.000      0.991 1.000 0.000
#&gt; GSM520698     1   0.482      0.891 0.896 0.104
#&gt; GSM520699     1   0.482      0.891 0.896 0.104
#&gt; GSM520700     1   0.482      0.891 0.896 0.104
#&gt; GSM520701     1   0.000      0.991 1.000 0.000
#&gt; GSM520702     1   0.000      0.991 1.000 0.000
#&gt; GSM520703     1   0.000      0.991 1.000 0.000
#&gt; GSM520671     1   0.000      0.991 1.000 0.000
#&gt; GSM520672     1   0.000      0.991 1.000 0.000
#&gt; GSM520673     1   0.000      0.991 1.000 0.000
#&gt; GSM520681     1   0.000      0.991 1.000 0.000
#&gt; GSM520682     1   0.000      0.991 1.000 0.000
#&gt; GSM520680     1   0.000      0.991 1.000 0.000
#&gt; GSM520677     1   0.000      0.991 1.000 0.000
#&gt; GSM520678     1   0.000      0.991 1.000 0.000
#&gt; GSM520679     1   0.000      0.991 1.000 0.000
#&gt; GSM520674     1   0.000      0.991 1.000 0.000
#&gt; GSM520675     1   0.000      0.991 1.000 0.000
#&gt; GSM520676     1   0.000      0.991 1.000 0.000
#&gt; GSM520686     1   0.000      0.991 1.000 0.000
#&gt; GSM520687     1   0.000      0.991 1.000 0.000
#&gt; GSM520688     1   0.000      0.991 1.000 0.000
#&gt; GSM520683     1   0.000      0.991 1.000 0.000
#&gt; GSM520684     1   0.000      0.991 1.000 0.000
#&gt; GSM520685     1   0.000      0.991 1.000 0.000
#&gt; GSM520708     1   0.000      0.991 1.000 0.000
#&gt; GSM520706     1   0.000      0.991 1.000 0.000
#&gt; GSM520707     1   0.000      0.991 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520704     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520705     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520711     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520692     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520689     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM520668     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM520669     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM520670     2  0.0237      0.996 0.004 0.996 0.000
#&gt; GSM520713     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520714     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520715     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520695     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520696     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520697     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520709     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520710     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520712     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520698     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520699     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520700     3  0.3551      0.848 0.132 0.000 0.868
#&gt; GSM520701     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520702     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520703     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520671     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520672     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520673     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520681     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM520708     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520706     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM520707     3  0.0000      0.992 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM520668     3  0.0188      0.987 0.000 0.004 0.996 0.000
#&gt; GSM520669     3  0.0188      0.987 0.000 0.004 0.996 0.000
#&gt; GSM520670     3  0.0188      0.987 0.000 0.004 0.996 0.000
#&gt; GSM520713     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520714     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520715     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520695     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520696     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520697     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520709     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520710     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520712     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520698     3  0.0817      0.980 0.000 0.000 0.976 0.024
#&gt; GSM520699     3  0.0817      0.980 0.000 0.000 0.976 0.024
#&gt; GSM520700     3  0.0336      0.987 0.000 0.000 0.992 0.008
#&gt; GSM520701     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520702     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520703     4  0.0000      0.995 0.000 0.000 0.000 1.000
#&gt; GSM520671     1  0.0707      0.990 0.980 0.000 0.020 0.000
#&gt; GSM520672     1  0.0707      0.990 0.980 0.000 0.020 0.000
#&gt; GSM520673     1  0.0707      0.990 0.980 0.000 0.020 0.000
#&gt; GSM520681     1  0.0000      0.991 1.000 0.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.991 1.000 0.000 0.000 0.000
#&gt; GSM520680     1  0.0707      0.990 0.980 0.000 0.020 0.000
#&gt; GSM520677     1  0.0188      0.991 0.996 0.000 0.004 0.000
#&gt; GSM520678     1  0.0188      0.991 0.996 0.000 0.004 0.000
#&gt; GSM520679     1  0.0188      0.991 0.996 0.000 0.004 0.000
#&gt; GSM520674     1  0.0188      0.991 0.996 0.000 0.004 0.000
#&gt; GSM520675     1  0.0000      0.991 1.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0188      0.991 0.996 0.000 0.004 0.000
#&gt; GSM520686     1  0.0592      0.990 0.984 0.000 0.016 0.000
#&gt; GSM520687     1  0.0592      0.990 0.984 0.000 0.016 0.000
#&gt; GSM520688     1  0.0592      0.990 0.984 0.000 0.016 0.000
#&gt; GSM520683     1  0.0000      0.991 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0592      0.990 0.984 0.000 0.016 0.000
#&gt; GSM520685     1  0.0336      0.991 0.992 0.000 0.008 0.000
#&gt; GSM520708     4  0.0779      0.982 0.016 0.000 0.004 0.980
#&gt; GSM520706     4  0.0779      0.982 0.016 0.000 0.004 0.980
#&gt; GSM520707     4  0.0779      0.982 0.016 0.000 0.004 0.980
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520704     2  0.0703      0.983 0.000 0.976 0.000 0.000 0.024
#&gt; GSM520705     2  0.0703      0.983 0.000 0.976 0.000 0.000 0.024
#&gt; GSM520711     2  0.0703      0.983 0.000 0.976 0.000 0.000 0.024
#&gt; GSM520692     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.3003      0.901 0.000 0.000 0.812 0.000 0.188
#&gt; GSM520669     3  0.3003      0.901 0.000 0.000 0.812 0.000 0.188
#&gt; GSM520670     3  0.3003      0.901 0.000 0.000 0.812 0.000 0.188
#&gt; GSM520713     4  0.0000      0.721 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520714     4  0.0000      0.721 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520715     4  0.0000      0.721 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520695     4  0.3513      0.580 0.000 0.000 0.180 0.800 0.020
#&gt; GSM520696     4  0.3513      0.580 0.000 0.000 0.180 0.800 0.020
#&gt; GSM520697     4  0.3513      0.580 0.000 0.000 0.180 0.800 0.020
#&gt; GSM520709     4  0.0510      0.712 0.000 0.000 0.000 0.984 0.016
#&gt; GSM520710     4  0.0510      0.712 0.000 0.000 0.000 0.984 0.016
#&gt; GSM520712     4  0.0000      0.721 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520698     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520699     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520700     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520701     4  0.3586      0.125 0.000 0.000 0.000 0.736 0.264
#&gt; GSM520702     4  0.3586      0.125 0.000 0.000 0.000 0.736 0.264
#&gt; GSM520703     4  0.3586      0.125 0.000 0.000 0.000 0.736 0.264
#&gt; GSM520671     1  0.2773      0.871 0.836 0.000 0.000 0.000 0.164
#&gt; GSM520672     1  0.2773      0.871 0.836 0.000 0.000 0.000 0.164
#&gt; GSM520673     1  0.2773      0.871 0.836 0.000 0.000 0.000 0.164
#&gt; GSM520681     1  0.1851      0.848 0.912 0.000 0.000 0.000 0.088
#&gt; GSM520682     1  0.1851      0.848 0.912 0.000 0.000 0.000 0.088
#&gt; GSM520680     1  0.2773      0.871 0.836 0.000 0.000 0.000 0.164
#&gt; GSM520677     1  0.3876      0.840 0.684 0.000 0.000 0.000 0.316
#&gt; GSM520678     1  0.3876      0.840 0.684 0.000 0.000 0.000 0.316
#&gt; GSM520679     1  0.3876      0.840 0.684 0.000 0.000 0.000 0.316
#&gt; GSM520674     1  0.3876      0.840 0.684 0.000 0.000 0.000 0.316
#&gt; GSM520675     1  0.3274      0.851 0.780 0.000 0.000 0.000 0.220
#&gt; GSM520676     1  0.3876      0.840 0.684 0.000 0.000 0.000 0.316
#&gt; GSM520686     1  0.0000      0.862 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.862 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.862 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.1851      0.848 0.912 0.000 0.000 0.000 0.088
#&gt; GSM520684     1  0.0290      0.859 0.992 0.000 0.000 0.000 0.008
#&gt; GSM520685     1  0.0290      0.859 0.992 0.000 0.000 0.000 0.008
#&gt; GSM520708     5  0.5046      1.000 0.032 0.000 0.000 0.468 0.500
#&gt; GSM520706     5  0.5046      1.000 0.032 0.000 0.000 0.468 0.500
#&gt; GSM520707     5  0.5046      1.000 0.032 0.000 0.000 0.468 0.500
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520704     2  0.2956   0.882842 0.000 0.848 0.000 0.000 0.088 0.064
#&gt; GSM520705     2  0.2956   0.882842 0.000 0.848 0.000 0.000 0.088 0.064
#&gt; GSM520711     2  0.2956   0.882842 0.000 0.848 0.000 0.000 0.088 0.064
#&gt; GSM520692     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000   0.963175 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.0146   0.772001 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM520669     3  0.0146   0.772001 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM520670     3  0.0146   0.772001 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; GSM520713     4  0.0000   0.663993 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520714     4  0.0000   0.663993 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520715     4  0.0146   0.662843 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM520695     4  0.4147   0.490594 0.000 0.000 0.004 0.668 0.024 0.304
#&gt; GSM520696     4  0.4147   0.490594 0.000 0.000 0.004 0.668 0.024 0.304
#&gt; GSM520697     4  0.4147   0.490594 0.000 0.000 0.004 0.668 0.024 0.304
#&gt; GSM520709     4  0.0891   0.648725 0.000 0.000 0.000 0.968 0.008 0.024
#&gt; GSM520710     4  0.0891   0.648725 0.000 0.000 0.000 0.968 0.008 0.024
#&gt; GSM520712     4  0.0000   0.663993 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520698     3  0.5406   0.773162 0.000 0.000 0.568 0.000 0.160 0.272
#&gt; GSM520699     3  0.5406   0.773162 0.000 0.000 0.568 0.000 0.160 0.272
#&gt; GSM520700     3  0.5406   0.773162 0.000 0.000 0.568 0.000 0.160 0.272
#&gt; GSM520701     4  0.3741   0.000662 0.000 0.000 0.000 0.672 0.008 0.320
#&gt; GSM520702     4  0.3741   0.000662 0.000 0.000 0.000 0.672 0.008 0.320
#&gt; GSM520703     4  0.3741   0.000662 0.000 0.000 0.000 0.672 0.008 0.320
#&gt; GSM520671     1  0.3774   0.196653 0.592 0.000 0.000 0.000 0.408 0.000
#&gt; GSM520672     1  0.3774   0.196653 0.592 0.000 0.000 0.000 0.408 0.000
#&gt; GSM520673     1  0.3774   0.196653 0.592 0.000 0.000 0.000 0.408 0.000
#&gt; GSM520681     5  0.4252   0.775419 0.372 0.000 0.000 0.000 0.604 0.024
#&gt; GSM520682     5  0.4252   0.775419 0.372 0.000 0.000 0.000 0.604 0.024
#&gt; GSM520680     1  0.3774   0.196653 0.592 0.000 0.000 0.000 0.408 0.000
#&gt; GSM520677     1  0.0000   0.666886 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000   0.666886 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000   0.666886 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000   0.666886 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.2491   0.445986 0.836 0.000 0.000 0.000 0.164 0.000
#&gt; GSM520676     1  0.0000   0.666886 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM520686     5  0.3309   0.877587 0.280 0.000 0.000 0.000 0.720 0.000
#&gt; GSM520687     5  0.3309   0.877587 0.280 0.000 0.000 0.000 0.720 0.000
#&gt; GSM520688     5  0.3309   0.877587 0.280 0.000 0.000 0.000 0.720 0.000
#&gt; GSM520683     5  0.4230   0.785333 0.364 0.000 0.000 0.000 0.612 0.024
#&gt; GSM520684     5  0.3309   0.877587 0.280 0.000 0.000 0.000 0.720 0.000
#&gt; GSM520685     5  0.3309   0.877587 0.280 0.000 0.000 0.000 0.720 0.000
#&gt; GSM520708     6  0.4721   1.000000 0.024 0.000 0.000 0.364 0.020 0.592
#&gt; GSM520706     6  0.4721   1.000000 0.024 0.000 0.000 0.364 0.020 0.592
#&gt; GSM520707     6  0.4721   1.000000 0.024 0.000 0.000 0.364 0.020 0.592
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n cell.type(p) cell.line(p) other(p) k
#> MAD:skmeans 51     7.71e-09     9.31e-07 2.23e-03 2
#> MAD:skmeans 51     6.64e-09     3.77e-10 4.74e-07 3
#> MAD:skmeans 51     4.89e-11     6.96e-12 5.84e-10 4
#> MAD:skmeans 48     9.44e-10     1.36e-14 7.29e-13 5
#> MAD:skmeans 40     1.49e-07     4.75e-14 6.38e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.914           0.940       0.974         0.7665 0.725   0.566
#> 4 4 1.000           0.930       0.964         0.0923 0.859   0.639
#> 5 5 1.000           0.932       0.971         0.0368 0.979   0.925
#> 6 6 0.816           0.758       0.884         0.0942 0.890   0.606
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520704     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520705     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520711     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520668     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520669     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520670     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520713     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520714     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520715     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520695     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520696     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520697     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520709     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520710     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520712     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520698     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520699     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520700     1   0.556      0.614 0.700  0 0.300
#&gt; GSM520701     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520702     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520703     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520671     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520672     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520673     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520681     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520682     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520680     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520677     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520678     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520679     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520674     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520675     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520676     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520686     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520687     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520688     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520683     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520684     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520685     1   0.000      0.942 1.000  0 0.000
#&gt; GSM520708     1   0.622      0.328 0.568  0 0.432
#&gt; GSM520706     1   0.556      0.614 0.700  0 0.300
#&gt; GSM520707     1   0.556      0.614 0.700  0 0.300
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520666     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520667     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520704     2   0.228      0.929 0.000 0.904 0.096 0.000
#&gt; GSM520705     2   0.228      0.929 0.000 0.904 0.096 0.000
#&gt; GSM520711     2   0.228      0.929 0.000 0.904 0.096 0.000
#&gt; GSM520692     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520693     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520694     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520689     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520690     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520691     2   0.000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM520668     3   0.228      1.000 0.096 0.000 0.904 0.000
#&gt; GSM520669     3   0.228      1.000 0.096 0.000 0.904 0.000
#&gt; GSM520670     3   0.228      1.000 0.096 0.000 0.904 0.000
#&gt; GSM520713     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520714     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520715     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520695     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520696     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520697     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520709     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520710     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520712     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520698     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520699     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520700     3   0.228      1.000 0.096 0.000 0.904 0.000
#&gt; GSM520701     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520702     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520703     4   0.000      0.901 0.000 0.000 0.000 1.000
#&gt; GSM520671     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520672     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520673     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520681     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520682     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520680     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520677     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520678     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520679     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520674     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520675     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520676     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520686     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520687     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520688     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520683     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520684     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520685     1   0.000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM520708     4   0.468      0.483 0.352 0.000 0.000 0.648
#&gt; GSM520706     4   0.483      0.417 0.392 0.000 0.000 0.608
#&gt; GSM520707     4   0.492      0.350 0.424 0.000 0.000 0.576
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520704     5  0.0794      1.000 0.000 0.028 0.000 0.000 0.972
#&gt; GSM520705     5  0.0794      1.000 0.000 0.028 0.000 0.000 0.972
#&gt; GSM520711     5  0.0794      1.000 0.000 0.028 0.000 0.000 0.972
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520669     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520670     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000
#&gt; GSM520713     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520714     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520715     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520695     4  0.0794      0.881 0.000 0.000 0.000 0.972 0.028
#&gt; GSM520696     4  0.0794      0.881 0.000 0.000 0.000 0.972 0.028
#&gt; GSM520697     4  0.0794      0.881 0.000 0.000 0.000 0.972 0.028
#&gt; GSM520709     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520710     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520712     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520698     4  0.0794      0.881 0.000 0.000 0.000 0.972 0.028
#&gt; GSM520699     4  0.0794      0.881 0.000 0.000 0.000 0.972 0.028
#&gt; GSM520700     3  0.0794      0.967 0.000 0.000 0.972 0.000 0.028
#&gt; GSM520701     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520702     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520703     4  0.0000      0.889 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520671     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.0703      0.975 0.976 0.000 0.000 0.000 0.024
#&gt; GSM520682     1  0.0162      0.995 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520680     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520708     4  0.4126      0.453 0.380 0.000 0.000 0.620 0.000
#&gt; GSM520706     4  0.4101      0.469 0.372 0.000 0.000 0.628 0.000
#&gt; GSM520707     4  0.4235      0.347 0.424 0.000 0.000 0.576 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4 p5    p6
#&gt; GSM520665     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520704     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM520705     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM520711     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM520668     3  0.0000      0.904 0.000  0 1.000 0.000  0 0.000
#&gt; GSM520669     3  0.0000      0.904 0.000  0 1.000 0.000  0 0.000
#&gt; GSM520670     3  0.0000      0.904 0.000  0 1.000 0.000  0 0.000
#&gt; GSM520713     4  0.0000      0.931 0.000  0 0.000 1.000  0 0.000
#&gt; GSM520714     4  0.0000      0.931 0.000  0 0.000 1.000  0 0.000
#&gt; GSM520715     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520695     4  0.2454      0.873 0.000  0 0.000 0.840  0 0.160
#&gt; GSM520696     4  0.2454      0.873 0.000  0 0.000 0.840  0 0.160
#&gt; GSM520697     4  0.2454      0.873 0.000  0 0.000 0.840  0 0.160
#&gt; GSM520709     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520710     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520712     4  0.0000      0.931 0.000  0 0.000 1.000  0 0.000
#&gt; GSM520698     4  0.2454      0.873 0.000  0 0.000 0.840  0 0.160
#&gt; GSM520699     4  0.2454      0.873 0.000  0 0.000 0.840  0 0.160
#&gt; GSM520700     3  0.3244      0.696 0.000  0 0.732 0.000  0 0.268
#&gt; GSM520701     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520702     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520703     4  0.0260      0.931 0.000  0 0.000 0.992  0 0.008
#&gt; GSM520671     1  0.1141      0.741 0.948  0 0.000 0.000  0 0.052
#&gt; GSM520672     6  0.3804      0.209 0.424  0 0.000 0.000  0 0.576
#&gt; GSM520673     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520681     6  0.3857      0.174 0.468  0 0.000 0.000  0 0.532
#&gt; GSM520682     1  0.3868     -0.271 0.508  0 0.000 0.000  0 0.492
#&gt; GSM520680     1  0.3592      0.247 0.656  0 0.000 0.000  0 0.344
#&gt; GSM520677     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520678     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520679     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520674     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520675     1  0.0713      0.757 0.972  0 0.000 0.000  0 0.028
#&gt; GSM520676     1  0.0000      0.778 1.000  0 0.000 0.000  0 0.000
#&gt; GSM520686     6  0.2527      0.640 0.168  0 0.000 0.000  0 0.832
#&gt; GSM520687     6  0.2527      0.640 0.168  0 0.000 0.000  0 0.832
#&gt; GSM520688     6  0.2527      0.640 0.168  0 0.000 0.000  0 0.832
#&gt; GSM520683     1  0.3867     -0.267 0.512  0 0.000 0.000  0 0.488
#&gt; GSM520684     6  0.2527      0.640 0.168  0 0.000 0.000  0 0.832
#&gt; GSM520685     6  0.2527      0.640 0.168  0 0.000 0.000  0 0.832
#&gt; GSM520708     6  0.6034      0.344 0.272  0 0.000 0.308  0 0.420
#&gt; GSM520706     6  0.5787      0.369 0.180  0 0.000 0.376  0 0.444
#&gt; GSM520707     6  0.5896      0.338 0.324  0 0.000 0.220  0 0.456
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n cell.type(p) cell.line(p) other(p) k
#> MAD:pam 51     1.46e-11     9.31e-07 5.89e-03 2
#> MAD:pam 50     1.39e-11     1.89e-10 1.02e-06 3
#> MAD:pam 48     2.13e-10     9.62e-13 8.09e-10 4
#> MAD:pam 48     9.44e-10     5.17e-17 4.55e-11 5
#> MAD:pam 43     3.70e-08     6.65e-19 1.87e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.995       0.997         0.3701 0.633   0.633
#> 3 3 1.000           0.999       1.000         0.7936 0.704   0.532
#> 4 4 0.914           0.950       0.971         0.0907 0.944   0.832
#> 5 5 0.896           0.820       0.929         0.0372 0.965   0.879
#> 6 6 0.889           0.844       0.906         0.0424 0.970   0.886
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM520665     2    0.00      1.000 0.000 1.000
#&gt; GSM520666     2    0.00      1.000 0.000 1.000
#&gt; GSM520667     2    0.00      1.000 0.000 1.000
#&gt; GSM520704     2    0.00      1.000 0.000 1.000
#&gt; GSM520705     2    0.00      1.000 0.000 1.000
#&gt; GSM520711     2    0.00      1.000 0.000 1.000
#&gt; GSM520692     2    0.00      1.000 0.000 1.000
#&gt; GSM520693     2    0.00      1.000 0.000 1.000
#&gt; GSM520694     2    0.00      1.000 0.000 1.000
#&gt; GSM520689     2    0.00      1.000 0.000 1.000
#&gt; GSM520690     2    0.00      1.000 0.000 1.000
#&gt; GSM520691     2    0.00      1.000 0.000 1.000
#&gt; GSM520668     1    0.26      0.956 0.956 0.044
#&gt; GSM520669     1    0.26      0.956 0.956 0.044
#&gt; GSM520670     1    0.26      0.956 0.956 0.044
#&gt; GSM520713     1    0.00      0.997 1.000 0.000
#&gt; GSM520714     1    0.00      0.997 1.000 0.000
#&gt; GSM520715     1    0.00      0.997 1.000 0.000
#&gt; GSM520695     1    0.00      0.997 1.000 0.000
#&gt; GSM520696     1    0.00      0.997 1.000 0.000
#&gt; GSM520697     1    0.00      0.997 1.000 0.000
#&gt; GSM520709     1    0.00      0.997 1.000 0.000
#&gt; GSM520710     1    0.00      0.997 1.000 0.000
#&gt; GSM520712     1    0.00      0.997 1.000 0.000
#&gt; GSM520698     1    0.00      0.997 1.000 0.000
#&gt; GSM520699     1    0.00      0.997 1.000 0.000
#&gt; GSM520700     1    0.00      0.997 1.000 0.000
#&gt; GSM520701     1    0.00      0.997 1.000 0.000
#&gt; GSM520702     1    0.00      0.997 1.000 0.000
#&gt; GSM520703     1    0.00      0.997 1.000 0.000
#&gt; GSM520671     1    0.00      0.997 1.000 0.000
#&gt; GSM520672     1    0.00      0.997 1.000 0.000
#&gt; GSM520673     1    0.00      0.997 1.000 0.000
#&gt; GSM520681     1    0.00      0.997 1.000 0.000
#&gt; GSM520682     1    0.00      0.997 1.000 0.000
#&gt; GSM520680     1    0.00      0.997 1.000 0.000
#&gt; GSM520677     1    0.00      0.997 1.000 0.000
#&gt; GSM520678     1    0.00      0.997 1.000 0.000
#&gt; GSM520679     1    0.00      0.997 1.000 0.000
#&gt; GSM520674     1    0.00      0.997 1.000 0.000
#&gt; GSM520675     1    0.00      0.997 1.000 0.000
#&gt; GSM520676     1    0.00      0.997 1.000 0.000
#&gt; GSM520686     1    0.00      0.997 1.000 0.000
#&gt; GSM520687     1    0.00      0.997 1.000 0.000
#&gt; GSM520688     1    0.00      0.997 1.000 0.000
#&gt; GSM520683     1    0.00      0.997 1.000 0.000
#&gt; GSM520684     1    0.00      0.997 1.000 0.000
#&gt; GSM520685     1    0.00      0.997 1.000 0.000
#&gt; GSM520708     1    0.00      0.997 1.000 0.000
#&gt; GSM520706     1    0.00      0.997 1.000 0.000
#&gt; GSM520707     1    0.00      0.997 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1    p2    p3
#&gt; GSM520665     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520666     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520667     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520704     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520705     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520711     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520692     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520693     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520694     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520689     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520690     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520691     2  0.0000      1.000  0 1.000 0.000
#&gt; GSM520668     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520669     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520670     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520713     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520714     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520715     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520695     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520696     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520697     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520709     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520710     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520712     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520698     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520699     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520700     3  0.0237      0.997  0 0.004 0.996
#&gt; GSM520701     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520702     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520703     3  0.0000      0.999  0 0.000 1.000
#&gt; GSM520671     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520672     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520673     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520681     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520682     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520680     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520677     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520678     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520679     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520674     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520675     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520676     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520686     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520687     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520688     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520683     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520684     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520685     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520708     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520706     1  0.0000      1.000  1 0.000 0.000
#&gt; GSM520707     1  0.0000      1.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520704     2  0.0592      0.989 0.000 0.984 0.016 0.000
#&gt; GSM520705     2  0.0592      0.989 0.000 0.984 0.016 0.000
#&gt; GSM520711     2  0.0592      0.989 0.000 0.984 0.016 0.000
#&gt; GSM520692     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.996 0.000 1.000 0.000 0.000
#&gt; GSM520668     3  0.0592      0.976 0.000 0.000 0.984 0.016
#&gt; GSM520669     3  0.0592      0.976 0.000 0.000 0.984 0.016
#&gt; GSM520670     3  0.0592      0.976 0.000 0.000 0.984 0.016
#&gt; GSM520713     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520714     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520715     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520695     4  0.3486      0.813 0.000 0.000 0.188 0.812
#&gt; GSM520696     4  0.3486      0.813 0.000 0.000 0.188 0.812
#&gt; GSM520697     4  0.3486      0.813 0.000 0.000 0.188 0.812
#&gt; GSM520709     4  0.1637      0.909 0.000 0.000 0.060 0.940
#&gt; GSM520710     4  0.1867      0.903 0.000 0.000 0.072 0.928
#&gt; GSM520712     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520698     3  0.1557      0.968 0.000 0.000 0.944 0.056
#&gt; GSM520699     3  0.1557      0.968 0.000 0.000 0.944 0.056
#&gt; GSM520700     3  0.1211      0.976 0.000 0.000 0.960 0.040
#&gt; GSM520701     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520702     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520703     4  0.0000      0.928 0.000 0.000 0.000 1.000
#&gt; GSM520671     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.0188      0.969 0.996 0.000 0.000 0.004
#&gt; GSM520682     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.972 1.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.3400      0.811 0.820 0.000 0.000 0.180
#&gt; GSM520706     1  0.3356      0.816 0.824 0.000 0.000 0.176
#&gt; GSM520707     1  0.3356      0.816 0.824 0.000 0.000 0.176
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520704     5  0.1270     1.0000 0.000 0.052 0.000 0.000 0.948
#&gt; GSM520705     5  0.1270     1.0000 0.000 0.052 0.000 0.000 0.948
#&gt; GSM520711     5  0.1270     1.0000 0.000 0.052 0.000 0.000 0.948
#&gt; GSM520692     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.0290     0.7773 0.000 0.000 0.992 0.000 0.008
#&gt; GSM520669     3  0.0290     0.7773 0.000 0.000 0.992 0.000 0.008
#&gt; GSM520670     3  0.0290     0.7773 0.000 0.000 0.992 0.000 0.008
#&gt; GSM520713     4  0.1410     0.8175 0.000 0.000 0.060 0.940 0.000
#&gt; GSM520714     4  0.1410     0.8175 0.000 0.000 0.060 0.940 0.000
#&gt; GSM520715     4  0.0000     0.8530 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520695     4  0.4307    -0.1974 0.000 0.000 0.500 0.500 0.000
#&gt; GSM520696     3  0.4307    -0.0291 0.000 0.000 0.500 0.500 0.000
#&gt; GSM520697     4  0.4307    -0.1974 0.000 0.000 0.500 0.500 0.000
#&gt; GSM520709     4  0.0162     0.8539 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520710     4  0.0162     0.8539 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520712     4  0.0000     0.8530 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520698     3  0.2648     0.7988 0.000 0.000 0.848 0.152 0.000
#&gt; GSM520699     3  0.2648     0.7988 0.000 0.000 0.848 0.152 0.000
#&gt; GSM520700     3  0.2648     0.7964 0.000 0.000 0.848 0.152 0.000
#&gt; GSM520701     4  0.0162     0.8539 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520702     4  0.0162     0.8539 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520703     4  0.0162     0.8539 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520671     1  0.0162     0.9251 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520672     1  0.0162     0.9251 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520673     1  0.0162     0.9251 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520681     1  0.0703     0.9112 0.976 0.000 0.000 0.000 0.024
#&gt; GSM520682     1  0.0290     0.9223 0.992 0.000 0.000 0.000 0.008
#&gt; GSM520680     1  0.0162     0.9251 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520677     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0000     0.9266 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520708     1  0.5759     0.4261 0.568 0.000 0.000 0.324 0.108
#&gt; GSM520706     1  0.5759     0.4261 0.568 0.000 0.000 0.324 0.108
#&gt; GSM520707     1  0.5759     0.4261 0.568 0.000 0.000 0.324 0.108
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520704     5  0.0000     1.0000 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM520705     5  0.0000     1.0000 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM520711     5  0.0000     1.0000 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM520692     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM520668     3  0.3266     1.0000 0.000  0 0.728 0.000 0.000 0.272
#&gt; GSM520669     3  0.3266     1.0000 0.000  0 0.728 0.000 0.000 0.272
#&gt; GSM520670     3  0.3266     1.0000 0.000  0 0.728 0.000 0.000 0.272
#&gt; GSM520713     4  0.1863     0.8735 0.000  0 0.000 0.896 0.000 0.104
#&gt; GSM520714     4  0.1863     0.8735 0.000  0 0.000 0.896 0.000 0.104
#&gt; GSM520715     4  0.0146     0.9639 0.000  0 0.000 0.996 0.000 0.004
#&gt; GSM520695     6  0.3499     0.6879 0.000  0 0.000 0.320 0.000 0.680
#&gt; GSM520696     6  0.3499     0.6879 0.000  0 0.000 0.320 0.000 0.680
#&gt; GSM520697     6  0.3499     0.6879 0.000  0 0.000 0.320 0.000 0.680
#&gt; GSM520709     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520710     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520712     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520698     6  0.0000     0.6287 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM520699     6  0.0000     0.6287 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM520700     6  0.0000     0.6287 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM520701     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520702     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520703     4  0.0000     0.9664 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM520671     1  0.1957     0.8192 0.888  0 0.112 0.000 0.000 0.000
#&gt; GSM520672     1  0.1957     0.8192 0.888  0 0.112 0.000 0.000 0.000
#&gt; GSM520673     1  0.1957     0.8192 0.888  0 0.112 0.000 0.000 0.000
#&gt; GSM520681     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520682     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520680     1  0.1957     0.8192 0.888  0 0.112 0.000 0.000 0.000
#&gt; GSM520677     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520678     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520679     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520674     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520675     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520676     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.1327     0.8428 0.936  0 0.064 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000     0.8619 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.1327     0.8428 0.936  0 0.064 0.000 0.000 0.000
#&gt; GSM520683     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520684     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520685     1  0.0937     0.8565 0.960  0 0.040 0.000 0.000 0.000
#&gt; GSM520708     1  0.7123     0.0971 0.392  0 0.160 0.332 0.116 0.000
#&gt; GSM520706     1  0.7123     0.0971 0.392  0 0.160 0.332 0.116 0.000
#&gt; GSM520707     1  0.7123     0.0971 0.392  0 0.160 0.332 0.116 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> MAD:mclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> MAD:mclust 51     8.42e-12     1.37e-11 1.03e-06 3
#> MAD:mclust 51     4.89e-11     3.46e-13 3.99e-10 4
#> MAD:mclust 45     3.98e-09     3.67e-14 2.17e-10 5
#> MAD:mclust 48     3.55e-09     5.89e-17 3.79e-12 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.934       0.976         0.3926 0.613   0.613
#> 3 3 0.850           0.915       0.962         0.6925 0.693   0.511
#> 4 4 0.790           0.739       0.868         0.0758 0.947   0.846
#> 5 5 0.816           0.805       0.858         0.0654 0.855   0.589
#> 6 6 0.795           0.711       0.864         0.0360 0.984   0.939
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM520665     2   0.000      0.961 0.000 1.000
#&gt; GSM520666     2   0.000      0.961 0.000 1.000
#&gt; GSM520667     2   0.000      0.961 0.000 1.000
#&gt; GSM520704     2   0.000      0.961 0.000 1.000
#&gt; GSM520705     2   0.000      0.961 0.000 1.000
#&gt; GSM520711     2   0.000      0.961 0.000 1.000
#&gt; GSM520692     2   0.000      0.961 0.000 1.000
#&gt; GSM520693     2   0.000      0.961 0.000 1.000
#&gt; GSM520694     2   0.000      0.961 0.000 1.000
#&gt; GSM520689     2   0.000      0.961 0.000 1.000
#&gt; GSM520690     2   0.000      0.961 0.000 1.000
#&gt; GSM520691     2   0.000      0.961 0.000 1.000
#&gt; GSM520668     1   0.958      0.364 0.620 0.380
#&gt; GSM520669     1   0.958      0.364 0.620 0.380
#&gt; GSM520670     2   0.992      0.144 0.448 0.552
#&gt; GSM520713     1   0.000      0.978 1.000 0.000
#&gt; GSM520714     1   0.000      0.978 1.000 0.000
#&gt; GSM520715     1   0.000      0.978 1.000 0.000
#&gt; GSM520695     1   0.000      0.978 1.000 0.000
#&gt; GSM520696     1   0.000      0.978 1.000 0.000
#&gt; GSM520697     1   0.000      0.978 1.000 0.000
#&gt; GSM520709     1   0.000      0.978 1.000 0.000
#&gt; GSM520710     1   0.000      0.978 1.000 0.000
#&gt; GSM520712     1   0.000      0.978 1.000 0.000
#&gt; GSM520698     1   0.000      0.978 1.000 0.000
#&gt; GSM520699     1   0.000      0.978 1.000 0.000
#&gt; GSM520700     1   0.000      0.978 1.000 0.000
#&gt; GSM520701     1   0.000      0.978 1.000 0.000
#&gt; GSM520702     1   0.000      0.978 1.000 0.000
#&gt; GSM520703     1   0.000      0.978 1.000 0.000
#&gt; GSM520671     1   0.000      0.978 1.000 0.000
#&gt; GSM520672     1   0.000      0.978 1.000 0.000
#&gt; GSM520673     1   0.000      0.978 1.000 0.000
#&gt; GSM520681     1   0.000      0.978 1.000 0.000
#&gt; GSM520682     1   0.000      0.978 1.000 0.000
#&gt; GSM520680     1   0.000      0.978 1.000 0.000
#&gt; GSM520677     1   0.000      0.978 1.000 0.000
#&gt; GSM520678     1   0.000      0.978 1.000 0.000
#&gt; GSM520679     1   0.000      0.978 1.000 0.000
#&gt; GSM520674     1   0.000      0.978 1.000 0.000
#&gt; GSM520675     1   0.000      0.978 1.000 0.000
#&gt; GSM520676     1   0.000      0.978 1.000 0.000
#&gt; GSM520686     1   0.000      0.978 1.000 0.000
#&gt; GSM520687     1   0.000      0.978 1.000 0.000
#&gt; GSM520688     1   0.000      0.978 1.000 0.000
#&gt; GSM520683     1   0.000      0.978 1.000 0.000
#&gt; GSM520684     1   0.000      0.978 1.000 0.000
#&gt; GSM520685     1   0.000      0.978 1.000 0.000
#&gt; GSM520708     1   0.000      0.978 1.000 0.000
#&gt; GSM520706     1   0.000      0.978 1.000 0.000
#&gt; GSM520707     1   0.000      0.978 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1   p2    p3
#&gt; GSM520665     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000 1.00 0.000
#&gt; GSM520668     1  0.5560      0.600 0.700 0.30 0.000
#&gt; GSM520669     1  0.4291      0.773 0.820 0.18 0.000
#&gt; GSM520670     1  0.5397      0.635 0.720 0.28 0.000
#&gt; GSM520713     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520714     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520715     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520695     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520696     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520697     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520709     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520710     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520712     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520698     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520699     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520700     3  0.6309     -0.129 0.496 0.00 0.504
#&gt; GSM520701     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520702     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520703     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520671     1  0.0000      0.924 1.000 0.00 0.000
#&gt; GSM520672     1  0.0000      0.924 1.000 0.00 0.000
#&gt; GSM520673     1  0.0000      0.924 1.000 0.00 0.000
#&gt; GSM520681     1  0.3482      0.860 0.872 0.00 0.128
#&gt; GSM520682     1  0.3340      0.868 0.880 0.00 0.120
#&gt; GSM520680     1  0.0000      0.924 1.000 0.00 0.000
#&gt; GSM520677     1  0.0747      0.923 0.984 0.00 0.016
#&gt; GSM520678     1  0.0424      0.925 0.992 0.00 0.008
#&gt; GSM520679     1  0.0237      0.925 0.996 0.00 0.004
#&gt; GSM520674     1  0.0237      0.925 0.996 0.00 0.004
#&gt; GSM520675     1  0.3116      0.877 0.892 0.00 0.108
#&gt; GSM520676     1  0.0237      0.925 0.996 0.00 0.004
#&gt; GSM520686     1  0.0237      0.925 0.996 0.00 0.004
#&gt; GSM520687     1  0.0237      0.925 0.996 0.00 0.004
#&gt; GSM520688     1  0.0424      0.925 0.992 0.00 0.008
#&gt; GSM520683     1  0.3340      0.868 0.880 0.00 0.120
#&gt; GSM520684     1  0.1753      0.911 0.952 0.00 0.048
#&gt; GSM520685     1  0.2959      0.883 0.900 0.00 0.100
#&gt; GSM520708     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520706     3  0.0000      0.967 0.000 0.00 1.000
#&gt; GSM520707     3  0.0000      0.967 0.000 0.00 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0188      0.775 0.000 0.996 0.004 0.000
#&gt; GSM520666     2  0.0188      0.775 0.000 0.996 0.004 0.000
#&gt; GSM520667     2  0.0188      0.775 0.000 0.996 0.004 0.000
#&gt; GSM520704     2  0.4679      0.607 0.000 0.648 0.352 0.000
#&gt; GSM520705     2  0.4679      0.607 0.000 0.648 0.352 0.000
#&gt; GSM520711     2  0.4679      0.607 0.000 0.648 0.352 0.000
#&gt; GSM520692     2  0.2589      0.743 0.000 0.884 0.116 0.000
#&gt; GSM520693     2  0.1389      0.771 0.000 0.952 0.048 0.000
#&gt; GSM520694     2  0.1118      0.774 0.000 0.964 0.036 0.000
#&gt; GSM520689     2  0.0336      0.777 0.000 0.992 0.008 0.000
#&gt; GSM520690     2  0.0000      0.776 0.000 1.000 0.000 0.000
#&gt; GSM520691     2  0.0336      0.772 0.000 0.992 0.008 0.000
#&gt; GSM520668     2  0.8064     -0.907 0.004 0.348 0.300 0.348
#&gt; GSM520669     3  0.8048      0.989 0.004 0.348 0.360 0.288
#&gt; GSM520670     3  0.8041      0.989 0.004 0.348 0.364 0.284
#&gt; GSM520713     4  0.0000      0.792 0.000 0.000 0.000 1.000
#&gt; GSM520714     4  0.0000      0.792 0.000 0.000 0.000 1.000
#&gt; GSM520715     4  0.0000      0.792 0.000 0.000 0.000 1.000
#&gt; GSM520695     4  0.0469      0.785 0.000 0.000 0.012 0.988
#&gt; GSM520696     4  0.0469      0.785 0.000 0.000 0.012 0.988
#&gt; GSM520697     4  0.0188      0.789 0.000 0.000 0.004 0.996
#&gt; GSM520709     4  0.0336      0.790 0.008 0.000 0.000 0.992
#&gt; GSM520710     4  0.0336      0.790 0.008 0.000 0.000 0.992
#&gt; GSM520712     4  0.0188      0.792 0.004 0.000 0.000 0.996
#&gt; GSM520698     4  0.3791      0.532 0.000 0.004 0.200 0.796
#&gt; GSM520699     4  0.3791      0.532 0.000 0.004 0.200 0.796
#&gt; GSM520700     4  0.5610      0.336 0.008 0.064 0.208 0.720
#&gt; GSM520701     4  0.0188      0.792 0.004 0.000 0.000 0.996
#&gt; GSM520702     4  0.0188      0.792 0.004 0.000 0.000 0.996
#&gt; GSM520703     4  0.0188      0.792 0.004 0.000 0.000 0.996
#&gt; GSM520671     1  0.4040      0.799 0.752 0.000 0.248 0.000
#&gt; GSM520672     1  0.4907      0.570 0.580 0.000 0.420 0.000
#&gt; GSM520673     1  0.2589      0.905 0.884 0.000 0.116 0.000
#&gt; GSM520681     1  0.0592      0.921 0.984 0.000 0.000 0.016
#&gt; GSM520682     1  0.0188      0.927 0.996 0.000 0.000 0.004
#&gt; GSM520680     1  0.3266      0.871 0.832 0.000 0.168 0.000
#&gt; GSM520677     1  0.2741      0.908 0.892 0.000 0.096 0.012
#&gt; GSM520678     1  0.1970      0.923 0.932 0.000 0.060 0.008
#&gt; GSM520679     1  0.1824      0.924 0.936 0.000 0.060 0.004
#&gt; GSM520674     1  0.1389      0.927 0.952 0.000 0.048 0.000
#&gt; GSM520675     1  0.0188      0.927 0.996 0.000 0.000 0.004
#&gt; GSM520676     1  0.1474      0.926 0.948 0.000 0.052 0.000
#&gt; GSM520686     1  0.0707      0.929 0.980 0.000 0.020 0.000
#&gt; GSM520687     1  0.0592      0.929 0.984 0.000 0.016 0.000
#&gt; GSM520688     1  0.0592      0.929 0.984 0.000 0.016 0.000
#&gt; GSM520683     1  0.0188      0.927 0.996 0.000 0.000 0.004
#&gt; GSM520684     1  0.1109      0.927 0.968 0.000 0.028 0.004
#&gt; GSM520685     1  0.0657      0.929 0.984 0.000 0.012 0.004
#&gt; GSM520708     4  0.4948      0.306 0.440 0.000 0.000 0.560
#&gt; GSM520706     4  0.4948      0.306 0.440 0.000 0.000 0.560
#&gt; GSM520707     4  0.4961      0.290 0.448 0.000 0.000 0.552
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0510    0.98003 0.000 0.984 0.016 0.000 0.000
#&gt; GSM520666     2  0.0510    0.98003 0.000 0.984 0.016 0.000 0.000
#&gt; GSM520667     2  0.0510    0.98003 0.000 0.984 0.016 0.000 0.000
#&gt; GSM520704     5  0.3561    1.00000 0.000 0.260 0.000 0.000 0.740
#&gt; GSM520705     5  0.3561    1.00000 0.000 0.260 0.000 0.000 0.740
#&gt; GSM520711     5  0.3561    1.00000 0.000 0.260 0.000 0.000 0.740
#&gt; GSM520692     2  0.0404    0.97471 0.000 0.988 0.000 0.000 0.012
#&gt; GSM520693     2  0.0404    0.97471 0.000 0.988 0.000 0.000 0.012
#&gt; GSM520694     2  0.0566    0.97730 0.000 0.984 0.004 0.000 0.012
#&gt; GSM520689     2  0.0000    0.98150 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000    0.98150 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520691     2  0.0510    0.98003 0.000 0.984 0.016 0.000 0.000
#&gt; GSM520668     3  0.1106    0.69065 0.000 0.012 0.964 0.024 0.000
#&gt; GSM520669     3  0.1012    0.68603 0.000 0.012 0.968 0.020 0.000
#&gt; GSM520670     3  0.1106    0.69065 0.000 0.012 0.964 0.024 0.000
#&gt; GSM520713     4  0.1197    0.95145 0.000 0.000 0.048 0.952 0.000
#&gt; GSM520714     4  0.1197    0.95145 0.000 0.000 0.048 0.952 0.000
#&gt; GSM520715     4  0.1121    0.95263 0.000 0.000 0.044 0.956 0.000
#&gt; GSM520695     4  0.0671    0.96745 0.000 0.000 0.016 0.980 0.004
#&gt; GSM520696     4  0.0865    0.96091 0.000 0.000 0.024 0.972 0.004
#&gt; GSM520697     4  0.0290    0.97294 0.000 0.000 0.008 0.992 0.000
#&gt; GSM520709     4  0.0566    0.96377 0.012 0.000 0.004 0.984 0.000
#&gt; GSM520710     4  0.0566    0.96377 0.012 0.000 0.004 0.984 0.000
#&gt; GSM520712     4  0.0000    0.97303 0.000 0.000 0.000 1.000 0.000
#&gt; GSM520698     3  0.4449    0.58740 0.000 0.004 0.604 0.388 0.004
#&gt; GSM520699     3  0.4480    0.56981 0.000 0.004 0.592 0.400 0.004
#&gt; GSM520700     3  0.4040    0.69135 0.000 0.012 0.712 0.276 0.000
#&gt; GSM520701     4  0.0162    0.97370 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520702     4  0.0162    0.97370 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520703     4  0.0162    0.97370 0.000 0.000 0.000 0.996 0.004
#&gt; GSM520671     1  0.5068    0.65602 0.640 0.000 0.300 0.000 0.060
#&gt; GSM520672     1  0.5129    0.63336 0.616 0.000 0.328 0.000 0.056
#&gt; GSM520673     1  0.4380    0.70298 0.708 0.000 0.260 0.000 0.032
#&gt; GSM520681     1  0.0486    0.77387 0.988 0.000 0.004 0.004 0.004
#&gt; GSM520682     1  0.0771    0.77728 0.976 0.000 0.020 0.000 0.004
#&gt; GSM520680     1  0.4866    0.67294 0.664 0.000 0.284 0.000 0.052
#&gt; GSM520677     1  0.5696    0.68171 0.712 0.000 0.096 0.096 0.096
#&gt; GSM520678     1  0.3479    0.75896 0.836 0.000 0.080 0.000 0.084
#&gt; GSM520679     1  0.3479    0.75966 0.836 0.000 0.084 0.000 0.080
#&gt; GSM520674     1  0.3301    0.76224 0.848 0.000 0.080 0.000 0.072
#&gt; GSM520675     1  0.0451    0.77353 0.988 0.000 0.008 0.000 0.004
#&gt; GSM520676     1  0.3354    0.76300 0.844 0.000 0.088 0.000 0.068
#&gt; GSM520686     1  0.2390    0.77123 0.896 0.000 0.084 0.000 0.020
#&gt; GSM520687     1  0.2331    0.77208 0.900 0.000 0.080 0.000 0.020
#&gt; GSM520688     1  0.2079    0.77344 0.916 0.000 0.064 0.000 0.020
#&gt; GSM520683     1  0.0771    0.77039 0.976 0.000 0.004 0.000 0.020
#&gt; GSM520684     1  0.3639    0.72593 0.792 0.000 0.184 0.000 0.024
#&gt; GSM520685     1  0.2012    0.77365 0.920 0.000 0.060 0.000 0.020
#&gt; GSM520708     1  0.5427   -0.00325 0.480 0.000 0.020 0.476 0.024
#&gt; GSM520706     1  0.5383    0.17931 0.536 0.000 0.020 0.420 0.024
#&gt; GSM520707     1  0.5330    0.25561 0.564 0.000 0.020 0.392 0.024
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.0937     0.9714 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM520666     2  0.0937     0.9714 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM520667     2  0.0937     0.9714 0.000 0.960 0.000 0.000 0.000 0.040
#&gt; GSM520704     5  0.1501     1.0000 0.000 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520705     5  0.1501     1.0000 0.000 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520711     5  0.1501     1.0000 0.000 0.076 0.000 0.000 0.924 0.000
#&gt; GSM520692     2  0.0291     0.9825 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; GSM520693     2  0.0146     0.9825 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520694     2  0.0000     0.9833 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     2  0.0146     0.9832 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520690     2  0.0146     0.9832 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520691     2  0.0146     0.9832 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM520668     3  0.0806     0.8731 0.000 0.000 0.972 0.020 0.000 0.008
#&gt; GSM520669     3  0.0622     0.8674 0.000 0.000 0.980 0.012 0.000 0.008
#&gt; GSM520670     3  0.0806     0.8731 0.000 0.000 0.972 0.020 0.000 0.008
#&gt; GSM520713     4  0.2488     0.8672 0.000 0.000 0.076 0.880 0.000 0.044
#&gt; GSM520714     4  0.2376     0.8731 0.000 0.000 0.068 0.888 0.000 0.044
#&gt; GSM520715     4  0.2563     0.8632 0.000 0.000 0.072 0.876 0.000 0.052
#&gt; GSM520695     4  0.0146     0.9168 0.000 0.000 0.004 0.996 0.000 0.000
#&gt; GSM520696     4  0.0260     0.9168 0.000 0.000 0.008 0.992 0.000 0.000
#&gt; GSM520697     4  0.0146     0.9168 0.000 0.000 0.004 0.996 0.000 0.000
#&gt; GSM520709     4  0.1080     0.9084 0.004 0.000 0.004 0.960 0.000 0.032
#&gt; GSM520710     4  0.1080     0.9084 0.004 0.000 0.004 0.960 0.000 0.032
#&gt; GSM520712     4  0.0260     0.9143 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM520698     3  0.4138     0.8324 0.000 0.000 0.772 0.112 0.016 0.100
#&gt; GSM520699     3  0.4138     0.8325 0.000 0.000 0.772 0.112 0.016 0.100
#&gt; GSM520700     3  0.2982     0.8683 0.000 0.000 0.860 0.060 0.012 0.068
#&gt; GSM520701     4  0.2466     0.8648 0.000 0.000 0.008 0.872 0.008 0.112
#&gt; GSM520702     4  0.2712     0.8620 0.000 0.000 0.016 0.864 0.012 0.108
#&gt; GSM520703     4  0.2666     0.8630 0.000 0.000 0.012 0.864 0.012 0.112
#&gt; GSM520671     1  0.3766     0.4677 0.720 0.000 0.024 0.000 0.000 0.256
#&gt; GSM520672     1  0.4272     0.4547 0.704 0.000 0.052 0.000 0.004 0.240
#&gt; GSM520673     1  0.3320     0.5313 0.772 0.000 0.016 0.000 0.000 0.212
#&gt; GSM520681     1  0.1444     0.6246 0.928 0.000 0.000 0.000 0.000 0.072
#&gt; GSM520682     1  0.1863     0.6144 0.896 0.000 0.000 0.000 0.000 0.104
#&gt; GSM520680     1  0.3619     0.5013 0.744 0.000 0.024 0.000 0.000 0.232
#&gt; GSM520677     6  0.6531     0.0000 0.332 0.000 0.016 0.256 0.004 0.392
#&gt; GSM520678     1  0.4896    -0.0830 0.528 0.000 0.016 0.032 0.000 0.424
#&gt; GSM520679     1  0.4592     0.0710 0.560 0.000 0.016 0.016 0.000 0.408
#&gt; GSM520674     1  0.4585     0.0859 0.564 0.000 0.016 0.016 0.000 0.404
#&gt; GSM520675     1  0.2340     0.6057 0.852 0.000 0.000 0.000 0.000 0.148
#&gt; GSM520676     1  0.4345     0.2718 0.624 0.000 0.020 0.008 0.000 0.348
#&gt; GSM520686     1  0.0508     0.6278 0.984 0.000 0.000 0.000 0.004 0.012
#&gt; GSM520687     1  0.0692     0.6283 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM520688     1  0.0692     0.6252 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM520683     1  0.0865     0.6307 0.964 0.000 0.000 0.000 0.000 0.036
#&gt; GSM520684     1  0.1088     0.6251 0.960 0.000 0.016 0.000 0.000 0.024
#&gt; GSM520685     1  0.1204     0.6036 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM520708     1  0.5322     0.1498 0.596 0.000 0.000 0.216 0.000 0.188
#&gt; GSM520706     1  0.4795     0.2918 0.672 0.000 0.000 0.152 0.000 0.176
#&gt; GSM520707     1  0.4693     0.3112 0.684 0.000 0.000 0.140 0.000 0.176
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n cell.type(p) cell.line(p) other(p) k
#> MAD:NMF 48     6.02e-11     1.43e-06 7.23e-03 2
#> MAD:NMF 50     1.39e-11     6.67e-10 1.12e-06 3
#> MAD:NMF 46     5.67e-10     7.30e-14 8.69e-11 4
#> MAD:NMF 48     9.44e-10     3.50e-16 1.08e-11 5
#> MAD:NMF 41     2.69e-08     3.20e-12 1.25e-09 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           0.978       0.990         0.2638 0.915   0.866
#> 4 4 0.968           0.960       0.969         0.0455 0.979   0.961
#> 5 5 1.000           0.978       0.990         0.0290 0.986   0.973
#> 6 6 1.000           1.000       1.000         0.1304 0.922   0.849
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520704     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520705     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520711     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000
#&gt; GSM520668     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520669     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520670     3   0.000      1.000 0.000  0 1.000
#&gt; GSM520713     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520714     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520715     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520695     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520696     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520697     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520709     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520710     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520712     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520698     1   0.424      0.799 0.824  0 0.176
#&gt; GSM520699     1   0.424      0.799 0.824  0 0.176
#&gt; GSM520700     1   0.424      0.799 0.824  0 0.176
#&gt; GSM520701     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520702     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520703     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520671     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520672     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520673     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520681     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520682     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520680     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520677     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520678     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520679     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520674     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520675     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520676     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520686     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520687     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520688     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520683     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520684     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520685     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520708     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520706     1   0.000      0.985 1.000  0 0.000
#&gt; GSM520707     1   0.000      0.985 1.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520666     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520667     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520704     4   0.331      1.000 0.000 0.172 0.000 0.828
#&gt; GSM520705     4   0.331      1.000 0.000 0.172 0.000 0.828
#&gt; GSM520711     4   0.331      1.000 0.000 0.172 0.000 0.828
#&gt; GSM520692     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520693     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520694     2   0.000      0.922 0.000 1.000 0.000 0.000
#&gt; GSM520689     2   0.331      0.847 0.000 0.828 0.000 0.172
#&gt; GSM520690     2   0.331      0.847 0.000 0.828 0.000 0.172
#&gt; GSM520691     2   0.331      0.847 0.000 0.828 0.000 0.172
#&gt; GSM520668     3   0.000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM520669     3   0.000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM520670     3   0.000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM520713     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520714     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520715     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520695     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520696     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520697     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520709     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520710     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520712     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520698     1   0.336      0.799 0.824 0.000 0.176 0.000
#&gt; GSM520699     1   0.336      0.799 0.824 0.000 0.176 0.000
#&gt; GSM520700     1   0.336      0.799 0.824 0.000 0.176 0.000
#&gt; GSM520701     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520702     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520703     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520671     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520672     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520673     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520681     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520682     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520680     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520677     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520678     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520679     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520674     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520675     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520676     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520686     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520687     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520688     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520683     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520684     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520685     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520708     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520706     1   0.000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM520707     1   0.000      0.985 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4 p5
#&gt; GSM520665     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520666     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520667     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520704     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520705     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520711     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520692     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520693     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520694     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520689     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520690     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520691     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520668     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520669     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520670     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520713     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520714     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520715     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520695     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520696     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520697     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520709     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520710     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520712     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520698     4   0.289      0.799  0  0 0.176 0.824  0
#&gt; GSM520699     4   0.289      0.799  0  0 0.176 0.824  0
#&gt; GSM520700     4   0.289      0.799  0  0 0.176 0.824  0
#&gt; GSM520701     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520702     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520703     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520671     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520672     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520673     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520681     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520682     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520680     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520677     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520678     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520679     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520674     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520675     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520676     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520686     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520687     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520688     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520683     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520684     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520685     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520708     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520706     4   0.000      0.985  0  0 0.000 1.000  0
#&gt; GSM520707     4   0.000      0.985  0  0 0.000 1.000  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3 p4 p5 p6
#&gt; GSM520665     2       0          1  0  1  0  0  0  0
#&gt; GSM520666     2       0          1  0  1  0  0  0  0
#&gt; GSM520667     2       0          1  0  1  0  0  0  0
#&gt; GSM520704     5       0          1  0  0  0  0  1  0
#&gt; GSM520705     5       0          1  0  0  0  0  1  0
#&gt; GSM520711     5       0          1  0  0  0  0  1  0
#&gt; GSM520692     2       0          1  0  1  0  0  0  0
#&gt; GSM520693     2       0          1  0  1  0  0  0  0
#&gt; GSM520694     2       0          1  0  1  0  0  0  0
#&gt; GSM520689     6       0          1  0  0  0  0  0  1
#&gt; GSM520690     6       0          1  0  0  0  0  0  1
#&gt; GSM520691     6       0          1  0  0  0  0  0  1
#&gt; GSM520668     3       0          1  0  0  1  0  0  0
#&gt; GSM520669     3       0          1  0  0  1  0  0  0
#&gt; GSM520670     3       0          1  0  0  1  0  0  0
#&gt; GSM520713     1       0          1  1  0  0  0  0  0
#&gt; GSM520714     1       0          1  1  0  0  0  0  0
#&gt; GSM520715     1       0          1  1  0  0  0  0  0
#&gt; GSM520695     1       0          1  1  0  0  0  0  0
#&gt; GSM520696     1       0          1  1  0  0  0  0  0
#&gt; GSM520697     1       0          1  1  0  0  0  0  0
#&gt; GSM520709     1       0          1  1  0  0  0  0  0
#&gt; GSM520710     1       0          1  1  0  0  0  0  0
#&gt; GSM520712     1       0          1  1  0  0  0  0  0
#&gt; GSM520698     4       0          1  0  0  0  1  0  0
#&gt; GSM520699     4       0          1  0  0  0  1  0  0
#&gt; GSM520700     4       0          1  0  0  0  1  0  0
#&gt; GSM520701     1       0          1  1  0  0  0  0  0
#&gt; GSM520702     1       0          1  1  0  0  0  0  0
#&gt; GSM520703     1       0          1  1  0  0  0  0  0
#&gt; GSM520671     1       0          1  1  0  0  0  0  0
#&gt; GSM520672     1       0          1  1  0  0  0  0  0
#&gt; GSM520673     1       0          1  1  0  0  0  0  0
#&gt; GSM520681     1       0          1  1  0  0  0  0  0
#&gt; GSM520682     1       0          1  1  0  0  0  0  0
#&gt; GSM520680     1       0          1  1  0  0  0  0  0
#&gt; GSM520677     1       0          1  1  0  0  0  0  0
#&gt; GSM520678     1       0          1  1  0  0  0  0  0
#&gt; GSM520679     1       0          1  1  0  0  0  0  0
#&gt; GSM520674     1       0          1  1  0  0  0  0  0
#&gt; GSM520675     1       0          1  1  0  0  0  0  0
#&gt; GSM520676     1       0          1  1  0  0  0  0  0
#&gt; GSM520686     1       0          1  1  0  0  0  0  0
#&gt; GSM520687     1       0          1  1  0  0  0  0  0
#&gt; GSM520688     1       0          1  1  0  0  0  0  0
#&gt; GSM520683     1       0          1  1  0  0  0  0  0
#&gt; GSM520684     1       0          1  1  0  0  0  0  0
#&gt; GSM520685     1       0          1  1  0  0  0  0  0
#&gt; GSM520708     1       0          1  1  0  0  0  0  0
#&gt; GSM520706     1       0          1  1  0  0  0  0  0
#&gt; GSM520707     1       0          1  1  0  0  0  0  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> ATC:hclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:hclust 51     8.42e-12     1.37e-11 2.17e-07 3
#> ATC:hclust 51     4.89e-11     2.26e-16 1.89e-08 4
#> ATC:hclust 51     2.23e-10     3.95e-21 2.43e-10 5
#> ATC:hclust 51     8.65e-10     2.84e-21 6.17e-12 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.619           0.739       0.869         0.4885 0.845   0.755
#> 4 4 0.628           0.785       0.800         0.2326 0.802   0.586
#> 5 5 0.690           0.836       0.770         0.1081 0.914   0.692
#> 6 6 0.673           0.810       0.758         0.0534 0.993   0.964
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.1289      0.963 0.000 0.968 0.032
#&gt; GSM520666     2  0.1289      0.963 0.000 0.968 0.032
#&gt; GSM520667     2  0.1289      0.963 0.000 0.968 0.032
#&gt; GSM520704     2  0.3116      0.948 0.000 0.892 0.108
#&gt; GSM520705     2  0.3116      0.948 0.000 0.892 0.108
#&gt; GSM520711     2  0.3116      0.948 0.000 0.892 0.108
#&gt; GSM520692     2  0.0000      0.963 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.963 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.963 0.000 1.000 0.000
#&gt; GSM520689     2  0.2356      0.950 0.000 0.928 0.072
#&gt; GSM520690     2  0.2356      0.950 0.000 0.928 0.072
#&gt; GSM520691     2  0.2356      0.950 0.000 0.928 0.072
#&gt; GSM520668     3  0.4121      0.848 0.168 0.000 0.832
#&gt; GSM520669     3  0.4121      0.848 0.168 0.000 0.832
#&gt; GSM520670     3  0.4121      0.848 0.168 0.000 0.832
#&gt; GSM520713     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520714     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520715     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520695     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520696     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520697     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520709     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520710     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520712     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520698     3  0.5905      0.764 0.352 0.000 0.648
#&gt; GSM520699     3  0.5905      0.764 0.352 0.000 0.648
#&gt; GSM520700     3  0.5431      0.801 0.284 0.000 0.716
#&gt; GSM520701     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520702     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520703     1  0.5760      0.464 0.672 0.000 0.328
#&gt; GSM520671     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520672     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520673     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520681     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520680     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520677     1  0.0592      0.757 0.988 0.000 0.012
#&gt; GSM520678     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520675     1  0.1411      0.751 0.964 0.000 0.036
#&gt; GSM520676     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520686     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520687     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520688     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520683     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520684     1  0.2261      0.734 0.932 0.000 0.068
#&gt; GSM520685     1  0.1411      0.751 0.964 0.000 0.036
#&gt; GSM520708     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.763 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.763 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.1256      0.946 0.000 0.964 0.028 0.008
#&gt; GSM520666     2  0.1256      0.946 0.000 0.964 0.028 0.008
#&gt; GSM520667     2  0.1256      0.946 0.000 0.964 0.028 0.008
#&gt; GSM520704     2  0.4292      0.898 0.000 0.820 0.080 0.100
#&gt; GSM520705     2  0.4292      0.898 0.000 0.820 0.080 0.100
#&gt; GSM520711     2  0.4300      0.898 0.000 0.820 0.088 0.092
#&gt; GSM520692     2  0.0000      0.946 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.946 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.946 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.1706      0.943 0.000 0.948 0.016 0.036
#&gt; GSM520690     2  0.1677      0.943 0.000 0.948 0.012 0.040
#&gt; GSM520691     2  0.1677      0.943 0.000 0.948 0.012 0.040
#&gt; GSM520668     3  0.3611      0.782 0.060 0.000 0.860 0.080
#&gt; GSM520669     3  0.3611      0.782 0.060 0.000 0.860 0.080
#&gt; GSM520670     3  0.3611      0.782 0.060 0.000 0.860 0.080
#&gt; GSM520713     4  0.3444      0.980 0.184 0.000 0.000 0.816
#&gt; GSM520714     4  0.3444      0.980 0.184 0.000 0.000 0.816
#&gt; GSM520715     4  0.3444      0.980 0.184 0.000 0.000 0.816
#&gt; GSM520695     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520696     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520697     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520709     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520710     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520712     4  0.3486      0.981 0.188 0.000 0.000 0.812
#&gt; GSM520698     3  0.6474      0.687 0.076 0.000 0.536 0.388
#&gt; GSM520699     3  0.6474      0.687 0.076 0.000 0.536 0.388
#&gt; GSM520700     3  0.6648      0.700 0.092 0.000 0.536 0.372
#&gt; GSM520701     4  0.3450      0.947 0.156 0.000 0.008 0.836
#&gt; GSM520702     4  0.3450      0.947 0.156 0.000 0.008 0.836
#&gt; GSM520703     4  0.3450      0.947 0.156 0.000 0.008 0.836
#&gt; GSM520671     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520681     1  0.5440      0.417 0.596 0.000 0.020 0.384
#&gt; GSM520682     1  0.5400      0.442 0.608 0.000 0.020 0.372
#&gt; GSM520680     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520677     1  0.5550      0.304 0.552 0.000 0.020 0.428
#&gt; GSM520678     1  0.4963      0.583 0.696 0.000 0.020 0.284
#&gt; GSM520679     1  0.4963      0.583 0.696 0.000 0.020 0.284
#&gt; GSM520674     1  0.4963      0.583 0.696 0.000 0.020 0.284
#&gt; GSM520675     1  0.0188      0.756 0.996 0.000 0.000 0.004
#&gt; GSM520676     1  0.2174      0.741 0.928 0.000 0.020 0.052
#&gt; GSM520686     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520683     1  0.2174      0.741 0.928 0.000 0.020 0.052
#&gt; GSM520684     1  0.0000      0.756 1.000 0.000 0.000 0.000
#&gt; GSM520685     1  0.0188      0.756 0.996 0.000 0.000 0.004
#&gt; GSM520708     1  0.5842      0.244 0.520 0.000 0.032 0.448
#&gt; GSM520706     1  0.5827      0.286 0.532 0.000 0.032 0.436
#&gt; GSM520707     1  0.5827      0.286 0.532 0.000 0.032 0.436
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.1770      0.905 0.048 0.936 0.008 0.008 0.000
#&gt; GSM520666     2  0.1770      0.905 0.048 0.936 0.008 0.008 0.000
#&gt; GSM520667     2  0.1770      0.905 0.048 0.936 0.008 0.008 0.000
#&gt; GSM520704     2  0.3828      0.854 0.220 0.764 0.008 0.008 0.000
#&gt; GSM520705     2  0.3828      0.854 0.220 0.764 0.008 0.008 0.000
#&gt; GSM520711     2  0.4048      0.855 0.208 0.764 0.012 0.016 0.000
#&gt; GSM520692     2  0.0162      0.906 0.000 0.996 0.000 0.004 0.000
#&gt; GSM520693     2  0.0162      0.906 0.000 0.996 0.000 0.004 0.000
#&gt; GSM520694     2  0.0162      0.906 0.000 0.996 0.000 0.004 0.000
#&gt; GSM520689     2  0.3130      0.877 0.096 0.856 0.000 0.048 0.000
#&gt; GSM520690     2  0.3130      0.877 0.096 0.856 0.000 0.048 0.000
#&gt; GSM520691     2  0.3130      0.877 0.096 0.856 0.000 0.048 0.000
#&gt; GSM520668     3  0.0854      0.719 0.008 0.000 0.976 0.004 0.012
#&gt; GSM520669     3  0.0854      0.719 0.008 0.000 0.976 0.004 0.012
#&gt; GSM520670     3  0.0968      0.719 0.012 0.000 0.972 0.004 0.012
#&gt; GSM520713     4  0.4302      0.874 0.048 0.000 0.000 0.744 0.208
#&gt; GSM520714     4  0.4302      0.874 0.048 0.000 0.000 0.744 0.208
#&gt; GSM520715     4  0.4302      0.874 0.048 0.000 0.000 0.744 0.208
#&gt; GSM520695     4  0.3684      0.859 0.000 0.000 0.000 0.720 0.280
#&gt; GSM520696     4  0.3684      0.859 0.000 0.000 0.000 0.720 0.280
#&gt; GSM520697     4  0.3684      0.859 0.000 0.000 0.000 0.720 0.280
#&gt; GSM520709     4  0.4073      0.881 0.032 0.000 0.000 0.752 0.216
#&gt; GSM520710     4  0.4073      0.881 0.032 0.000 0.000 0.752 0.216
#&gt; GSM520712     4  0.4073      0.881 0.032 0.000 0.000 0.752 0.216
#&gt; GSM520698     3  0.6910      0.603 0.084 0.000 0.476 0.372 0.068
#&gt; GSM520699     3  0.6910      0.603 0.084 0.000 0.476 0.372 0.068
#&gt; GSM520700     3  0.6894      0.611 0.092 0.000 0.476 0.372 0.060
#&gt; GSM520701     4  0.4355      0.737 0.076 0.000 0.000 0.760 0.164
#&gt; GSM520702     4  0.4355      0.737 0.076 0.000 0.000 0.760 0.164
#&gt; GSM520703     4  0.4355      0.737 0.076 0.000 0.000 0.760 0.164
#&gt; GSM520671     1  0.4294      0.989 0.532 0.000 0.000 0.000 0.468
#&gt; GSM520672     1  0.4294      0.989 0.532 0.000 0.000 0.000 0.468
#&gt; GSM520673     1  0.4294      0.989 0.532 0.000 0.000 0.000 0.468
#&gt; GSM520681     5  0.2280      0.803 0.000 0.000 0.000 0.120 0.880
#&gt; GSM520682     5  0.2230      0.804 0.000 0.000 0.000 0.116 0.884
#&gt; GSM520680     1  0.4294      0.989 0.532 0.000 0.000 0.000 0.468
#&gt; GSM520677     5  0.2561      0.789 0.000 0.000 0.000 0.144 0.856
#&gt; GSM520678     5  0.2124      0.779 0.028 0.000 0.000 0.056 0.916
#&gt; GSM520679     5  0.2124      0.779 0.028 0.000 0.000 0.056 0.916
#&gt; GSM520674     5  0.2124      0.779 0.028 0.000 0.000 0.056 0.916
#&gt; GSM520675     1  0.4297      0.984 0.528 0.000 0.000 0.000 0.472
#&gt; GSM520676     5  0.2338      0.534 0.112 0.000 0.000 0.004 0.884
#&gt; GSM520686     1  0.4283      0.988 0.544 0.000 0.000 0.000 0.456
#&gt; GSM520687     1  0.4283      0.988 0.544 0.000 0.000 0.000 0.456
#&gt; GSM520688     1  0.4283      0.988 0.544 0.000 0.000 0.000 0.456
#&gt; GSM520683     5  0.2179      0.528 0.112 0.000 0.000 0.000 0.888
#&gt; GSM520684     1  0.4283      0.988 0.544 0.000 0.000 0.000 0.456
#&gt; GSM520685     1  0.4291      0.986 0.536 0.000 0.000 0.000 0.464
#&gt; GSM520708     5  0.3953      0.760 0.048 0.000 0.000 0.168 0.784
#&gt; GSM520706     5  0.3914      0.763 0.048 0.000 0.000 0.164 0.788
#&gt; GSM520707     5  0.3914      0.763 0.048 0.000 0.000 0.164 0.788
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.2948      0.855 0.004 0.880 0.008 0.024 0.036 0.048
#&gt; GSM520666     2  0.2948      0.855 0.004 0.880 0.008 0.024 0.036 0.048
#&gt; GSM520667     2  0.2948      0.855 0.004 0.880 0.008 0.024 0.036 0.048
#&gt; GSM520704     2  0.4054      0.788 0.000 0.688 0.004 0.000 0.024 0.284
#&gt; GSM520705     2  0.4054      0.788 0.000 0.688 0.004 0.000 0.024 0.284
#&gt; GSM520711     2  0.4417      0.789 0.000 0.688 0.008 0.012 0.024 0.268
#&gt; GSM520692     2  0.0146      0.861 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520693     2  0.0146      0.861 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520694     2  0.0146      0.861 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM520689     2  0.3908      0.816 0.008 0.808 0.000 0.052 0.028 0.104
#&gt; GSM520690     2  0.3926      0.816 0.008 0.808 0.000 0.044 0.036 0.104
#&gt; GSM520691     2  0.3926      0.816 0.008 0.808 0.000 0.044 0.036 0.104
#&gt; GSM520668     3  0.0820      0.991 0.000 0.000 0.972 0.012 0.016 0.000
#&gt; GSM520669     3  0.0820      0.991 0.000 0.000 0.972 0.012 0.016 0.000
#&gt; GSM520670     3  0.1406      0.982 0.004 0.000 0.952 0.016 0.020 0.008
#&gt; GSM520713     4  0.4574      0.727 0.172 0.000 0.000 0.732 0.036 0.060
#&gt; GSM520714     4  0.4574      0.727 0.172 0.000 0.000 0.732 0.036 0.060
#&gt; GSM520715     4  0.4574      0.727 0.172 0.000 0.000 0.732 0.036 0.060
#&gt; GSM520695     4  0.5026      0.739 0.268 0.000 0.000 0.640 0.016 0.076
#&gt; GSM520696     4  0.5026      0.739 0.268 0.000 0.000 0.640 0.016 0.076
#&gt; GSM520697     4  0.5026      0.739 0.268 0.000 0.000 0.640 0.016 0.076
#&gt; GSM520709     4  0.3201      0.768 0.208 0.000 0.000 0.780 0.012 0.000
#&gt; GSM520710     4  0.3201      0.768 0.208 0.000 0.000 0.780 0.012 0.000
#&gt; GSM520712     4  0.3201      0.768 0.208 0.000 0.000 0.780 0.012 0.000
#&gt; GSM520698     6  0.6931      0.972 0.068 0.000 0.328 0.208 0.000 0.396
#&gt; GSM520699     6  0.6931      0.972 0.068 0.000 0.328 0.208 0.000 0.396
#&gt; GSM520700     6  0.7168      0.943 0.056 0.000 0.328 0.204 0.016 0.396
#&gt; GSM520701     4  0.5756      0.414 0.188 0.000 0.000 0.548 0.008 0.256
#&gt; GSM520702     4  0.5756      0.414 0.188 0.000 0.000 0.548 0.008 0.256
#&gt; GSM520703     4  0.5756      0.414 0.188 0.000 0.000 0.548 0.008 0.256
#&gt; GSM520671     5  0.2362      0.935 0.136 0.000 0.000 0.000 0.860 0.004
#&gt; GSM520672     5  0.2362      0.935 0.136 0.000 0.000 0.000 0.860 0.004
#&gt; GSM520673     5  0.2362      0.935 0.136 0.000 0.000 0.000 0.860 0.004
#&gt; GSM520681     1  0.3358      0.825 0.824 0.000 0.000 0.052 0.116 0.008
#&gt; GSM520682     1  0.3358      0.825 0.824 0.000 0.000 0.052 0.116 0.008
#&gt; GSM520680     5  0.2362      0.935 0.136 0.000 0.000 0.000 0.860 0.004
#&gt; GSM520677     1  0.3244      0.816 0.832 0.000 0.000 0.064 0.100 0.004
#&gt; GSM520678     1  0.3062      0.821 0.816 0.000 0.000 0.024 0.160 0.000
#&gt; GSM520679     1  0.3062      0.821 0.816 0.000 0.000 0.024 0.160 0.000
#&gt; GSM520674     1  0.3062      0.821 0.816 0.000 0.000 0.024 0.160 0.000
#&gt; GSM520675     5  0.2854      0.878 0.208 0.000 0.000 0.000 0.792 0.000
#&gt; GSM520676     1  0.3265      0.705 0.748 0.000 0.000 0.004 0.248 0.000
#&gt; GSM520686     5  0.3412      0.933 0.128 0.000 0.000 0.000 0.808 0.064
#&gt; GSM520687     5  0.3412      0.933 0.128 0.000 0.000 0.000 0.808 0.064
#&gt; GSM520688     5  0.3412      0.933 0.128 0.000 0.000 0.000 0.808 0.064
#&gt; GSM520683     1  0.3290      0.697 0.744 0.000 0.000 0.000 0.252 0.004
#&gt; GSM520684     5  0.3468      0.932 0.128 0.000 0.000 0.000 0.804 0.068
#&gt; GSM520685     5  0.4305      0.833 0.232 0.000 0.000 0.000 0.700 0.068
#&gt; GSM520708     1  0.3401      0.678 0.832 0.000 0.012 0.048 0.004 0.104
#&gt; GSM520706     1  0.3545      0.687 0.828 0.000 0.012 0.044 0.012 0.104
#&gt; GSM520707     1  0.3545      0.687 0.828 0.000 0.012 0.044 0.012 0.104
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> ATC:kmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:kmeans 39     3.40e-09     8.56e-09 2.66e-04 3
#> ATC:kmeans 45     9.25e-10     1.13e-10 1.23e-09 4
#> ATC:kmeans 51     2.23e-10     1.51e-13 1.34e-09 5
#> ATC:kmeans 48     3.55e-09     3.68e-19 5.53e-11 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000          0.368 0.633   0.633
#> 3 3 1.000           1.000       1.000          0.230 0.915   0.866
#> 4 4 1.000           0.993       0.995          0.541 0.753   0.549
#> 5 5 0.848           0.950       0.964          0.049 0.972   0.906
#> 6 6 0.908           0.945       0.941          0.100 0.914   0.684
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM520665     2       0          1  0  1  0
#&gt; GSM520666     2       0          1  0  1  0
#&gt; GSM520667     2       0          1  0  1  0
#&gt; GSM520704     2       0          1  0  1  0
#&gt; GSM520705     2       0          1  0  1  0
#&gt; GSM520711     2       0          1  0  1  0
#&gt; GSM520692     2       0          1  0  1  0
#&gt; GSM520693     2       0          1  0  1  0
#&gt; GSM520694     2       0          1  0  1  0
#&gt; GSM520689     2       0          1  0  1  0
#&gt; GSM520690     2       0          1  0  1  0
#&gt; GSM520691     2       0          1  0  1  0
#&gt; GSM520668     3       0          1  0  0  1
#&gt; GSM520669     3       0          1  0  0  1
#&gt; GSM520670     3       0          1  0  0  1
#&gt; GSM520713     1       0          1  1  0  0
#&gt; GSM520714     1       0          1  1  0  0
#&gt; GSM520715     1       0          1  1  0  0
#&gt; GSM520695     1       0          1  1  0  0
#&gt; GSM520696     1       0          1  1  0  0
#&gt; GSM520697     1       0          1  1  0  0
#&gt; GSM520709     1       0          1  1  0  0
#&gt; GSM520710     1       0          1  1  0  0
#&gt; GSM520712     1       0          1  1  0  0
#&gt; GSM520698     1       0          1  1  0  0
#&gt; GSM520699     1       0          1  1  0  0
#&gt; GSM520700     1       0          1  1  0  0
#&gt; GSM520701     1       0          1  1  0  0
#&gt; GSM520702     1       0          1  1  0  0
#&gt; GSM520703     1       0          1  1  0  0
#&gt; GSM520671     1       0          1  1  0  0
#&gt; GSM520672     1       0          1  1  0  0
#&gt; GSM520673     1       0          1  1  0  0
#&gt; GSM520681     1       0          1  1  0  0
#&gt; GSM520682     1       0          1  1  0  0
#&gt; GSM520680     1       0          1  1  0  0
#&gt; GSM520677     1       0          1  1  0  0
#&gt; GSM520678     1       0          1  1  0  0
#&gt; GSM520679     1       0          1  1  0  0
#&gt; GSM520674     1       0          1  1  0  0
#&gt; GSM520675     1       0          1  1  0  0
#&gt; GSM520676     1       0          1  1  0  0
#&gt; GSM520686     1       0          1  1  0  0
#&gt; GSM520687     1       0          1  1  0  0
#&gt; GSM520688     1       0          1  1  0  0
#&gt; GSM520683     1       0          1  1  0  0
#&gt; GSM520684     1       0          1  1  0  0
#&gt; GSM520685     1       0          1  1  0  0
#&gt; GSM520708     1       0          1  1  0  0
#&gt; GSM520706     1       0          1  1  0  0
#&gt; GSM520707     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4
#&gt; GSM520665     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520704     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520705     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520711     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520692     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520689     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520690     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520691     2  0.0000      1.000 0.000  1  0 0.000
#&gt; GSM520668     3  0.0000      1.000 0.000  0  1 0.000
#&gt; GSM520669     3  0.0000      1.000 0.000  0  1 0.000
#&gt; GSM520670     3  0.0000      1.000 0.000  0  1 0.000
#&gt; GSM520713     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520714     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520715     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520695     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520696     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520697     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520709     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520710     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520712     4  0.0707      0.987 0.020  0  0 0.980
#&gt; GSM520698     4  0.0000      0.979 0.000  0  0 1.000
#&gt; GSM520699     4  0.0000      0.979 0.000  0  0 1.000
#&gt; GSM520700     4  0.0817      0.966 0.024  0  0 0.976
#&gt; GSM520701     4  0.0000      0.979 0.000  0  0 1.000
#&gt; GSM520702     4  0.0000      0.979 0.000  0  0 1.000
#&gt; GSM520703     4  0.0000      0.979 0.000  0  0 1.000
#&gt; GSM520671     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520672     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520673     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520681     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520682     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520680     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520677     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520678     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520679     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520674     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520675     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520676     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520686     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520687     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520688     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520683     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520684     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520685     1  0.0000      0.996 1.000  0  0 0.000
#&gt; GSM520708     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520706     1  0.0336      0.994 0.992  0  0 0.008
#&gt; GSM520707     1  0.0336      0.994 0.992  0  0 0.008
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4    p5
#&gt; GSM520665     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520666     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520667     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520704     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520705     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520711     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520692     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520693     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520694     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520689     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520690     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520691     2   0.000      1.000 0.000  1  0 0.000 0.000
#&gt; GSM520668     3   0.000      1.000 0.000  0  1 0.000 0.000
#&gt; GSM520669     3   0.000      1.000 0.000  0  1 0.000 0.000
#&gt; GSM520670     3   0.000      1.000 0.000  0  1 0.000 0.000
#&gt; GSM520713     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520714     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520715     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520695     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520696     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520697     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520709     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520710     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520712     4   0.000      0.986 0.000  0  0 1.000 0.000
#&gt; GSM520698     5   0.120      0.951 0.000  0  0 0.048 0.952
#&gt; GSM520699     5   0.120      0.951 0.000  0  0 0.048 0.952
#&gt; GSM520700     5   0.000      0.901 0.000  0  0 0.000 1.000
#&gt; GSM520701     4   0.120      0.957 0.000  0  0 0.952 0.048
#&gt; GSM520702     4   0.120      0.957 0.000  0  0 0.952 0.048
#&gt; GSM520703     4   0.120      0.957 0.000  0  0 0.952 0.048
#&gt; GSM520671     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520672     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520673     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520681     1   0.238      0.885 0.872  0  0 0.128 0.000
#&gt; GSM520682     1   0.223      0.892 0.884  0  0 0.116 0.000
#&gt; GSM520680     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520677     1   0.242      0.881 0.868  0  0 0.132 0.000
#&gt; GSM520678     1   0.218      0.894 0.888  0  0 0.112 0.000
#&gt; GSM520679     1   0.218      0.894 0.888  0  0 0.112 0.000
#&gt; GSM520674     1   0.218      0.894 0.888  0  0 0.112 0.000
#&gt; GSM520675     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520676     1   0.051      0.908 0.984  0  0 0.016 0.000
#&gt; GSM520686     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520687     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520688     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520683     1   0.051      0.908 0.984  0  0 0.016 0.000
#&gt; GSM520684     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520685     1   0.120      0.910 0.952  0  0 0.000 0.048
#&gt; GSM520708     1   0.238      0.885 0.872  0  0 0.128 0.000
#&gt; GSM520706     1   0.238      0.885 0.872  0  0 0.128 0.000
#&gt; GSM520707     1   0.233      0.887 0.876  0  0 0.124 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; GSM520665     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520704     2  0.0146      0.997 0.004 0.996  0 0.000 0.000 0.000
#&gt; GSM520705     2  0.0146      0.997 0.004 0.996  0 0.000 0.000 0.000
#&gt; GSM520711     2  0.0146      0.997 0.004 0.996  0 0.000 0.000 0.000
#&gt; GSM520692     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520689     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520690     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520691     2  0.0000      0.999 0.000 1.000  0 0.000 0.000 0.000
#&gt; GSM520668     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM520669     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM520670     3  0.0000      1.000 0.000 0.000  1 0.000 0.000 0.000
#&gt; GSM520713     4  0.0363      0.923 0.012 0.000  0 0.988 0.000 0.000
#&gt; GSM520714     4  0.0363      0.923 0.012 0.000  0 0.988 0.000 0.000
#&gt; GSM520715     4  0.0363      0.923 0.012 0.000  0 0.988 0.000 0.000
#&gt; GSM520695     4  0.0632      0.921 0.024 0.000  0 0.976 0.000 0.000
#&gt; GSM520696     4  0.0632      0.921 0.024 0.000  0 0.976 0.000 0.000
#&gt; GSM520697     4  0.0632      0.921 0.024 0.000  0 0.976 0.000 0.000
#&gt; GSM520709     4  0.0146      0.924 0.004 0.000  0 0.996 0.000 0.000
#&gt; GSM520710     4  0.0146      0.924 0.004 0.000  0 0.996 0.000 0.000
#&gt; GSM520712     4  0.0146      0.924 0.004 0.000  0 0.996 0.000 0.000
#&gt; GSM520698     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM520699     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM520700     5  0.0000      1.000 0.000 0.000  0 0.000 1.000 0.000
#&gt; GSM520701     4  0.4012      0.773 0.164 0.000  0 0.752 0.084 0.000
#&gt; GSM520702     4  0.4012      0.773 0.164 0.000  0 0.752 0.084 0.000
#&gt; GSM520703     4  0.4012      0.773 0.164 0.000  0 0.752 0.084 0.000
#&gt; GSM520671     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520672     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520673     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520681     1  0.3541      0.913 0.748 0.000  0 0.020 0.000 0.232
#&gt; GSM520682     1  0.3541      0.913 0.748 0.000  0 0.020 0.000 0.232
#&gt; GSM520680     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520677     1  0.3514      0.913 0.752 0.000  0 0.020 0.000 0.228
#&gt; GSM520678     1  0.3368      0.914 0.756 0.000  0 0.012 0.000 0.232
#&gt; GSM520679     1  0.3368      0.914 0.756 0.000  0 0.012 0.000 0.232
#&gt; GSM520674     1  0.3368      0.914 0.756 0.000  0 0.012 0.000 0.232
#&gt; GSM520675     6  0.0260      0.990 0.008 0.000  0 0.000 0.000 0.992
#&gt; GSM520676     1  0.3373      0.902 0.744 0.000  0 0.008 0.000 0.248
#&gt; GSM520686     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520687     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520688     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520683     1  0.3314      0.895 0.740 0.000  0 0.004 0.000 0.256
#&gt; GSM520684     6  0.0000      0.998 0.000 0.000  0 0.000 0.000 1.000
#&gt; GSM520685     6  0.0146      0.995 0.004 0.000  0 0.000 0.000 0.996
#&gt; GSM520708     1  0.1075      0.785 0.952 0.000  0 0.000 0.000 0.048
#&gt; GSM520706     1  0.1075      0.785 0.952 0.000  0 0.000 0.000 0.048
#&gt; GSM520707     1  0.1075      0.785 0.952 0.000  0 0.000 0.000 0.048
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n cell.type(p) cell.line(p) other(p) k
#> ATC:skmeans 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:skmeans 51     8.42e-12     1.37e-11 2.17e-07 3
#> ATC:skmeans 51     4.89e-11     2.26e-16 4.62e-11 4
#> ATC:skmeans 51     2.23e-10     6.99e-16 5.39e-13 5
#> ATC:skmeans 51     8.65e-10     2.88e-16 1.93e-12 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 1.000           0.990       0.994         0.2403 0.915   0.866
#> 4 4 1.000           0.991       0.996         0.0494 0.979   0.961
#> 5 5 1.000           0.991       0.996         0.0295 0.986   0.973
#> 6 6 0.693           0.916       0.928         0.4452 0.753   0.518
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520704     2  0.0747      0.988 0.000 0.984 0.016
#&gt; GSM520705     2  0.0747      0.988 0.000 0.984 0.016
#&gt; GSM520711     2  0.0747      0.988 0.000 0.984 0.016
#&gt; GSM520692     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520689     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM520668     3  0.0747      1.000 0.016 0.000 0.984
#&gt; GSM520669     3  0.0747      1.000 0.016 0.000 0.984
#&gt; GSM520670     3  0.0747      1.000 0.016 0.000 0.984
#&gt; GSM520713     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520714     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520715     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520695     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520696     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520697     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520709     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520710     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520712     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520698     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520699     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520700     1  0.4399      0.764 0.812 0.000 0.188
#&gt; GSM520701     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520702     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520703     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520671     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520672     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520673     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520708     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520706     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM520707     1  0.0000      0.994 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3 p4
#&gt; GSM520665     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520666     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520667     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520704     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520705     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520711     4   0.000      1.000 0.000  0 0.000  1
#&gt; GSM520692     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520693     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520694     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520689     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520690     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520691     2   0.000      1.000 0.000  1 0.000  0
#&gt; GSM520668     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520669     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520670     3   0.000      1.000 0.000  0 1.000  0
#&gt; GSM520713     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520714     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520715     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520695     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520696     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520697     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520709     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520710     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520712     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520698     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520699     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520700     1   0.365      0.744 0.796  0 0.204  0
#&gt; GSM520701     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520702     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520703     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520671     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520672     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520673     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520681     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520682     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520680     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520677     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520678     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520679     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520674     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520675     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520676     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520686     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520687     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520688     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520683     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520684     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520685     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520708     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520706     1   0.000      0.994 1.000  0 0.000  0
#&gt; GSM520707     1   0.000      0.994 1.000  0 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4 p5
#&gt; GSM520665     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520666     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520667     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520704     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520705     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520711     5   0.000      1.000  0  0 0.000 0.000  1
#&gt; GSM520692     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520693     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520694     2   0.000      1.000  0  1 0.000 0.000  0
#&gt; GSM520689     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520690     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520691     1   0.000      1.000  1  0 0.000 0.000  0
#&gt; GSM520668     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520669     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520670     3   0.000      1.000  0  0 1.000 0.000  0
#&gt; GSM520713     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520714     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520715     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520695     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520696     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520697     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520709     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520710     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520712     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520698     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520699     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520700     4   0.314      0.744  0  0 0.204 0.796  0
#&gt; GSM520701     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520702     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520703     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520671     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520672     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520673     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520681     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520682     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520680     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520677     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520678     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520679     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520674     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520675     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520676     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520686     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520687     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520688     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520683     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520684     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520685     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520708     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520706     4   0.000      0.994  0  0 0.000 1.000  0
#&gt; GSM520707     4   0.000      0.994  0  0 0.000 1.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4 p5 p6
#&gt; GSM520665     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520666     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520667     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520704     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520705     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520711     5  0.0000      1.000 0.000  0  0 0.000  1  0
#&gt; GSM520692     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520693     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520694     2  0.0000      1.000 0.000  1  0 0.000  0  0
#&gt; GSM520689     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520690     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520691     6  0.0000      1.000 0.000  0  0 0.000  0  1
#&gt; GSM520668     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520669     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520670     3  0.0000      1.000 0.000  0  1 0.000  0  0
#&gt; GSM520713     4  0.0865      0.916 0.036  0  0 0.964  0  0
#&gt; GSM520714     4  0.2092      0.899 0.124  0  0 0.876  0  0
#&gt; GSM520715     4  0.2135      0.897 0.128  0  0 0.872  0  0
#&gt; GSM520695     4  0.2135      0.897 0.128  0  0 0.872  0  0
#&gt; GSM520696     4  0.2135      0.897 0.128  0  0 0.872  0  0
#&gt; GSM520697     4  0.2135      0.897 0.128  0  0 0.872  0  0
#&gt; GSM520709     4  0.0363      0.914 0.012  0  0 0.988  0  0
#&gt; GSM520710     4  0.1863      0.908 0.104  0  0 0.896  0  0
#&gt; GSM520712     4  0.1714      0.911 0.092  0  0 0.908  0  0
#&gt; GSM520698     4  0.0000      0.910 0.000  0  0 1.000  0  0
#&gt; GSM520699     4  0.0000      0.910 0.000  0  0 1.000  0  0
#&gt; GSM520700     4  0.0260      0.903 0.008  0  0 0.992  0  0
#&gt; GSM520701     4  0.0000      0.910 0.000  0  0 1.000  0  0
#&gt; GSM520702     4  0.0000      0.910 0.000  0  0 1.000  0  0
#&gt; GSM520703     4  0.0000      0.910 0.000  0  0 1.000  0  0
#&gt; GSM520671     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520672     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520673     1  0.1957      0.868 0.888  0  0 0.112  0  0
#&gt; GSM520681     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520682     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520680     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520677     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520678     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520679     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520674     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520675     1  0.0363      0.834 0.988  0  0 0.012  0  0
#&gt; GSM520676     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520686     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520687     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520688     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520683     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520684     1  0.0000      0.828 1.000  0  0 0.000  0  0
#&gt; GSM520685     1  0.0260      0.832 0.992  0  0 0.008  0  0
#&gt; GSM520708     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520706     1  0.2823      0.883 0.796  0  0 0.204  0  0
#&gt; GSM520707     1  0.2823      0.883 0.796  0  0 0.204  0  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n cell.type(p) cell.line(p) other(p) k
#> ATC:pam 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:pam 51     8.42e-12     1.37e-11 2.17e-07 3
#> ATC:pam 51     4.89e-11     2.26e-16 1.89e-08 4
#> ATC:pam 51     2.23e-10     3.95e-21 2.43e-10 5
#> ATC:pam 51     8.65e-10     7.10e-26 6.30e-14 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.969           0.952       0.977         0.4697 0.845   0.755
#> 4 4 0.616           0.671       0.837         0.1716 0.894   0.785
#> 5 5 0.771           0.820       0.893         0.1528 0.761   0.473
#> 6 6 0.797           0.864       0.909         0.0163 0.979   0.923
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520666     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520667     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520704     2  0.1031      0.984 0.000 0.976 0.024
#&gt; GSM520705     2  0.1031      0.984 0.000 0.976 0.024
#&gt; GSM520711     2  0.1031      0.984 0.000 0.976 0.024
#&gt; GSM520692     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520693     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520694     2  0.0000      0.992 0.000 1.000 0.000
#&gt; GSM520689     2  0.0424      0.991 0.000 0.992 0.008
#&gt; GSM520690     2  0.0424      0.991 0.000 0.992 0.008
#&gt; GSM520691     2  0.0424      0.991 0.000 0.992 0.008
#&gt; GSM520668     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM520669     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM520670     3  0.0000      0.978 0.000 0.000 1.000
#&gt; GSM520713     1  0.0237      0.965 0.996 0.000 0.004
#&gt; GSM520714     1  0.0237      0.965 0.996 0.000 0.004
#&gt; GSM520715     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520695     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520696     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520697     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520709     1  0.0237      0.965 0.996 0.000 0.004
#&gt; GSM520710     1  0.0424      0.963 0.992 0.000 0.008
#&gt; GSM520712     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520698     3  0.1031      0.978 0.024 0.000 0.976
#&gt; GSM520699     3  0.1031      0.978 0.024 0.000 0.976
#&gt; GSM520700     3  0.1031      0.978 0.024 0.000 0.976
#&gt; GSM520701     1  0.4504      0.774 0.804 0.000 0.196
#&gt; GSM520702     1  0.4178      0.804 0.828 0.000 0.172
#&gt; GSM520703     1  0.5859      0.518 0.656 0.000 0.344
#&gt; GSM520671     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520672     1  0.0237      0.965 0.996 0.000 0.004
#&gt; GSM520673     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520681     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520682     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520680     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520677     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520678     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520679     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520674     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520675     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520676     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520686     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520683     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520684     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM520708     1  0.0237      0.965 0.996 0.000 0.004
#&gt; GSM520706     1  0.2066      0.919 0.940 0.000 0.060
#&gt; GSM520707     1  0.4887      0.729 0.772 0.000 0.228
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM520665     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520666     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520667     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520704     3  0.1792      1.000 0.000 0.068 0.932 0.000
#&gt; GSM520705     3  0.1792      1.000 0.000 0.068 0.932 0.000
#&gt; GSM520711     3  0.1792      1.000 0.000 0.068 0.932 0.000
#&gt; GSM520692     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520693     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520694     2  0.0000      0.793 0.000 1.000 0.000 0.000
#&gt; GSM520689     2  0.5404      0.339 0.000 0.512 0.476 0.012
#&gt; GSM520690     2  0.5399      0.355 0.000 0.520 0.468 0.012
#&gt; GSM520691     2  0.5402      0.347 0.000 0.516 0.472 0.012
#&gt; GSM520668     4  0.4222      0.513 0.000 0.000 0.272 0.728
#&gt; GSM520669     4  0.4761      0.402 0.000 0.000 0.372 0.628
#&gt; GSM520670     4  0.4222      0.512 0.000 0.000 0.272 0.728
#&gt; GSM520713     1  0.4804      0.428 0.616 0.000 0.000 0.384
#&gt; GSM520714     1  0.4804      0.428 0.616 0.000 0.000 0.384
#&gt; GSM520715     1  0.4776      0.441 0.624 0.000 0.000 0.376
#&gt; GSM520695     1  0.2589      0.778 0.884 0.000 0.000 0.116
#&gt; GSM520696     1  0.2868      0.763 0.864 0.000 0.000 0.136
#&gt; GSM520697     1  0.2589      0.778 0.884 0.000 0.000 0.116
#&gt; GSM520709     1  0.4866      0.370 0.596 0.000 0.000 0.404
#&gt; GSM520710     1  0.4817      0.410 0.612 0.000 0.000 0.388
#&gt; GSM520712     1  0.4790      0.433 0.620 0.000 0.000 0.380
#&gt; GSM520698     4  0.0592      0.639 0.000 0.000 0.016 0.984
#&gt; GSM520699     4  0.0592      0.639 0.000 0.000 0.016 0.984
#&gt; GSM520700     4  0.0817      0.637 0.000 0.000 0.024 0.976
#&gt; GSM520701     4  0.4776      0.352 0.376 0.000 0.000 0.624
#&gt; GSM520702     4  0.4761      0.364 0.372 0.000 0.000 0.628
#&gt; GSM520703     4  0.4661      0.417 0.348 0.000 0.000 0.652
#&gt; GSM520671     1  0.1211      0.815 0.960 0.000 0.000 0.040
#&gt; GSM520672     1  0.2647      0.749 0.880 0.000 0.000 0.120
#&gt; GSM520673     1  0.1389      0.812 0.952 0.000 0.000 0.048
#&gt; GSM520681     1  0.0707      0.817 0.980 0.000 0.000 0.020
#&gt; GSM520682     1  0.0707      0.817 0.980 0.000 0.000 0.020
#&gt; GSM520680     1  0.1211      0.814 0.960 0.000 0.000 0.040
#&gt; GSM520677     1  0.0817      0.816 0.976 0.000 0.000 0.024
#&gt; GSM520678     1  0.0592      0.817 0.984 0.000 0.000 0.016
#&gt; GSM520679     1  0.0921      0.815 0.972 0.000 0.000 0.028
#&gt; GSM520674     1  0.0188      0.817 0.996 0.000 0.000 0.004
#&gt; GSM520675     1  0.1118      0.815 0.964 0.000 0.000 0.036
#&gt; GSM520676     1  0.0000      0.817 1.000 0.000 0.000 0.000
#&gt; GSM520686     1  0.1211      0.814 0.960 0.000 0.000 0.040
#&gt; GSM520687     1  0.1211      0.814 0.960 0.000 0.000 0.040
#&gt; GSM520688     1  0.1211      0.814 0.960 0.000 0.000 0.040
#&gt; GSM520683     1  0.0000      0.817 1.000 0.000 0.000 0.000
#&gt; GSM520684     1  0.1211      0.814 0.960 0.000 0.000 0.040
#&gt; GSM520685     1  0.1118      0.815 0.964 0.000 0.000 0.036
#&gt; GSM520708     1  0.3801      0.680 0.780 0.000 0.000 0.220
#&gt; GSM520706     1  0.4543      0.527 0.676 0.000 0.000 0.324
#&gt; GSM520707     1  0.4925      0.285 0.572 0.000 0.000 0.428
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520704     5  0.0451      0.721 0.000 0.008 0.004 0.000 0.988
#&gt; GSM520705     5  0.0451      0.721 0.000 0.008 0.004 0.000 0.988
#&gt; GSM520711     5  0.0451      0.721 0.000 0.008 0.004 0.000 0.988
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM520689     5  0.5820      0.632 0.012 0.080 0.328 0.000 0.580
#&gt; GSM520690     5  0.5820      0.632 0.012 0.080 0.328 0.000 0.580
#&gt; GSM520691     5  0.5869      0.629 0.012 0.084 0.328 0.000 0.576
#&gt; GSM520668     3  0.0162      0.691 0.000 0.000 0.996 0.000 0.004
#&gt; GSM520669     3  0.0162      0.691 0.000 0.000 0.996 0.000 0.004
#&gt; GSM520670     3  0.0162      0.691 0.000 0.000 0.996 0.000 0.004
#&gt; GSM520713     4  0.0992      0.881 0.008 0.000 0.024 0.968 0.000
#&gt; GSM520714     4  0.0992      0.881 0.008 0.000 0.024 0.968 0.000
#&gt; GSM520715     4  0.0898      0.882 0.008 0.000 0.020 0.972 0.000
#&gt; GSM520695     4  0.0880      0.880 0.032 0.000 0.000 0.968 0.000
#&gt; GSM520696     4  0.0324      0.884 0.004 0.000 0.004 0.992 0.000
#&gt; GSM520697     4  0.0880      0.880 0.032 0.000 0.000 0.968 0.000
#&gt; GSM520709     4  0.0992      0.882 0.008 0.000 0.024 0.968 0.000
#&gt; GSM520710     4  0.0992      0.882 0.008 0.000 0.024 0.968 0.000
#&gt; GSM520712     4  0.1082      0.882 0.008 0.000 0.028 0.964 0.000
#&gt; GSM520698     3  0.5158      0.697 0.308 0.000 0.640 0.040 0.012
#&gt; GSM520699     3  0.5158      0.697 0.308 0.000 0.640 0.040 0.012
#&gt; GSM520700     3  0.4684      0.697 0.308 0.000 0.664 0.016 0.012
#&gt; GSM520701     4  0.0740      0.884 0.008 0.000 0.008 0.980 0.004
#&gt; GSM520702     4  0.0740      0.884 0.008 0.000 0.008 0.980 0.004
#&gt; GSM520703     4  0.0740      0.884 0.008 0.000 0.008 0.980 0.004
#&gt; GSM520671     1  0.2690      0.823 0.844 0.000 0.000 0.156 0.000
#&gt; GSM520672     1  0.0963      0.927 0.964 0.000 0.000 0.036 0.000
#&gt; GSM520673     1  0.2732      0.814 0.840 0.000 0.000 0.160 0.000
#&gt; GSM520681     4  0.2886      0.818 0.148 0.000 0.000 0.844 0.008
#&gt; GSM520682     4  0.2929      0.817 0.152 0.000 0.000 0.840 0.008
#&gt; GSM520680     1  0.0794      0.929 0.972 0.000 0.000 0.028 0.000
#&gt; GSM520677     4  0.3455      0.768 0.208 0.000 0.000 0.784 0.008
#&gt; GSM520678     4  0.3910      0.682 0.272 0.000 0.000 0.720 0.008
#&gt; GSM520679     4  0.3093      0.805 0.168 0.000 0.000 0.824 0.008
#&gt; GSM520674     4  0.3353      0.785 0.196 0.000 0.000 0.796 0.008
#&gt; GSM520675     1  0.0609      0.934 0.980 0.000 0.000 0.020 0.000
#&gt; GSM520676     4  0.4278      0.329 0.452 0.000 0.000 0.548 0.000
#&gt; GSM520686     1  0.0609      0.934 0.980 0.000 0.000 0.020 0.000
#&gt; GSM520687     1  0.0609      0.934 0.980 0.000 0.000 0.020 0.000
#&gt; GSM520688     1  0.0609      0.934 0.980 0.000 0.000 0.020 0.000
#&gt; GSM520683     4  0.4283      0.321 0.456 0.000 0.000 0.544 0.000
#&gt; GSM520684     1  0.0609      0.934 0.980 0.000 0.000 0.020 0.000
#&gt; GSM520685     1  0.1851      0.883 0.912 0.000 0.000 0.088 0.000
#&gt; GSM520708     4  0.0693      0.883 0.008 0.000 0.000 0.980 0.012
#&gt; GSM520706     4  0.0579      0.883 0.008 0.000 0.000 0.984 0.008
#&gt; GSM520707     4  0.1299      0.881 0.020 0.000 0.008 0.960 0.012
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM520665     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520666     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520667     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520704     5  0.1444      1.000 0.000 0.000 0.000 0.000 0.928 0.072
#&gt; GSM520705     5  0.1444      1.000 0.000 0.000 0.000 0.000 0.928 0.072
#&gt; GSM520711     5  0.1444      1.000 0.000 0.000 0.000 0.000 0.928 0.072
#&gt; GSM520692     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520693     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520694     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM520689     6  0.0508      0.515 0.000 0.004 0.000 0.000 0.012 0.984
#&gt; GSM520690     6  0.0508      0.515 0.000 0.004 0.000 0.000 0.012 0.984
#&gt; GSM520691     6  0.0508      0.515 0.000 0.004 0.000 0.000 0.012 0.984
#&gt; GSM520668     3  0.0547      1.000 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM520669     3  0.0547      1.000 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM520670     3  0.0547      1.000 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM520713     4  0.0837      0.916 0.000 0.000 0.004 0.972 0.020 0.004
#&gt; GSM520714     4  0.0837      0.916 0.000 0.000 0.004 0.972 0.020 0.004
#&gt; GSM520715     4  0.0837      0.916 0.000 0.000 0.004 0.972 0.020 0.004
#&gt; GSM520695     4  0.0547      0.915 0.000 0.000 0.000 0.980 0.020 0.000
#&gt; GSM520696     4  0.0363      0.916 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; GSM520697     4  0.0547      0.915 0.000 0.000 0.000 0.980 0.020 0.000
#&gt; GSM520709     4  0.0692      0.916 0.000 0.000 0.004 0.976 0.020 0.000
#&gt; GSM520710     4  0.0692      0.916 0.000 0.000 0.004 0.976 0.020 0.000
#&gt; GSM520712     4  0.0692      0.916 0.000 0.000 0.004 0.976 0.020 0.000
#&gt; GSM520698     6  0.6591      0.427 0.316 0.000 0.292 0.024 0.000 0.368
#&gt; GSM520699     6  0.6591      0.427 0.316 0.000 0.292 0.024 0.000 0.368
#&gt; GSM520700     6  0.6683      0.422 0.316 0.000 0.292 0.012 0.012 0.368
#&gt; GSM520701     4  0.0806      0.916 0.000 0.000 0.008 0.972 0.020 0.000
#&gt; GSM520702     4  0.0806      0.916 0.000 0.000 0.008 0.972 0.020 0.000
#&gt; GSM520703     4  0.0806      0.916 0.000 0.000 0.008 0.972 0.020 0.000
#&gt; GSM520671     1  0.2346      0.825 0.868 0.000 0.000 0.124 0.008 0.000
#&gt; GSM520672     1  0.0820      0.941 0.972 0.000 0.000 0.016 0.012 0.000
#&gt; GSM520673     1  0.2389      0.818 0.864 0.000 0.000 0.128 0.008 0.000
#&gt; GSM520681     4  0.2867      0.883 0.076 0.000 0.016 0.872 0.032 0.004
#&gt; GSM520682     4  0.2921      0.881 0.080 0.000 0.016 0.868 0.032 0.004
#&gt; GSM520680     1  0.0458      0.946 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM520677     4  0.2658      0.885 0.072 0.000 0.016 0.884 0.024 0.004
#&gt; GSM520678     4  0.2829      0.872 0.096 0.000 0.016 0.864 0.024 0.000
#&gt; GSM520679     4  0.2649      0.884 0.076 0.000 0.016 0.880 0.028 0.000
#&gt; GSM520674     4  0.2647      0.879 0.088 0.000 0.016 0.876 0.020 0.000
#&gt; GSM520675     1  0.0458      0.946 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM520676     4  0.4430      0.523 0.344 0.000 0.016 0.624 0.016 0.000
#&gt; GSM520686     1  0.0458      0.946 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM520687     1  0.0458      0.946 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM520688     1  0.0458      0.946 0.984 0.000 0.000 0.016 0.000 0.000
#&gt; GSM520683     4  0.4430      0.523 0.344 0.000 0.016 0.624 0.016 0.000
#&gt; GSM520684     1  0.0717      0.942 0.976 0.000 0.000 0.016 0.008 0.000
#&gt; GSM520685     1  0.1265      0.923 0.948 0.000 0.000 0.044 0.008 0.000
#&gt; GSM520708     4  0.1036      0.913 0.000 0.000 0.008 0.964 0.024 0.004
#&gt; GSM520706     4  0.1036      0.913 0.000 0.000 0.008 0.964 0.024 0.004
#&gt; GSM520707     4  0.1294      0.913 0.008 0.000 0.008 0.956 0.024 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n cell.type(p) cell.line(p) other(p) k
#> ATC:mclust 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:mclust 51     8.42e-12     4.61e-09 4.66e-06 3
#> ATC:mclust 37     4.60e-08     3.16e-11 6.99e-06 4
#> ATC:mclust 49     5.84e-10     4.07e-14 1.80e-08 5
#> ATC:mclust 48     3.55e-09     3.42e-20 3.32e-12 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 42764 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3677 0.633   0.633
#> 3 3 0.638           0.762       0.742         0.4266 0.725   0.566
#> 4 4 0.564           0.712       0.797         0.1426 0.788   0.537
#> 5 5 0.559           0.688       0.728         0.0843 0.962   0.891
#> 6 6 0.634           0.776       0.849         0.0864 0.846   0.587
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM520665     2       0          1  0  1
#&gt; GSM520666     2       0          1  0  1
#&gt; GSM520667     2       0          1  0  1
#&gt; GSM520704     2       0          1  0  1
#&gt; GSM520705     2       0          1  0  1
#&gt; GSM520711     2       0          1  0  1
#&gt; GSM520692     2       0          1  0  1
#&gt; GSM520693     2       0          1  0  1
#&gt; GSM520694     2       0          1  0  1
#&gt; GSM520689     2       0          1  0  1
#&gt; GSM520690     2       0          1  0  1
#&gt; GSM520691     2       0          1  0  1
#&gt; GSM520668     1       0          1  1  0
#&gt; GSM520669     1       0          1  1  0
#&gt; GSM520670     1       0          1  1  0
#&gt; GSM520713     1       0          1  1  0
#&gt; GSM520714     1       0          1  1  0
#&gt; GSM520715     1       0          1  1  0
#&gt; GSM520695     1       0          1  1  0
#&gt; GSM520696     1       0          1  1  0
#&gt; GSM520697     1       0          1  1  0
#&gt; GSM520709     1       0          1  1  0
#&gt; GSM520710     1       0          1  1  0
#&gt; GSM520712     1       0          1  1  0
#&gt; GSM520698     1       0          1  1  0
#&gt; GSM520699     1       0          1  1  0
#&gt; GSM520700     1       0          1  1  0
#&gt; GSM520701     1       0          1  1  0
#&gt; GSM520702     1       0          1  1  0
#&gt; GSM520703     1       0          1  1  0
#&gt; GSM520671     1       0          1  1  0
#&gt; GSM520672     1       0          1  1  0
#&gt; GSM520673     1       0          1  1  0
#&gt; GSM520681     1       0          1  1  0
#&gt; GSM520682     1       0          1  1  0
#&gt; GSM520680     1       0          1  1  0
#&gt; GSM520677     1       0          1  1  0
#&gt; GSM520678     1       0          1  1  0
#&gt; GSM520679     1       0          1  1  0
#&gt; GSM520674     1       0          1  1  0
#&gt; GSM520675     1       0          1  1  0
#&gt; GSM520676     1       0          1  1  0
#&gt; GSM520686     1       0          1  1  0
#&gt; GSM520687     1       0          1  1  0
#&gt; GSM520688     1       0          1  1  0
#&gt; GSM520683     1       0          1  1  0
#&gt; GSM520684     1       0          1  1  0
#&gt; GSM520685     1       0          1  1  0
#&gt; GSM520708     1       0          1  1  0
#&gt; GSM520706     1       0          1  1  0
#&gt; GSM520707     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM520665     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520666     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520667     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520704     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520705     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520711     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520692     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520693     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520694     2  0.0237     0.9987 0.000 0.996 0.004
#&gt; GSM520689     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520690     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520691     2  0.0000     0.9987 0.000 1.000 0.000
#&gt; GSM520668     1  0.2356     0.7284 0.928 0.000 0.072
#&gt; GSM520669     1  0.2165     0.7339 0.936 0.000 0.064
#&gt; GSM520670     1  0.2261     0.7314 0.932 0.000 0.068
#&gt; GSM520713     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520714     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520715     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520695     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520696     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520697     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520709     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520710     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520712     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520698     3  0.6267     0.9514 0.452 0.000 0.548
#&gt; GSM520699     3  0.6267     0.9514 0.452 0.000 0.548
#&gt; GSM520700     1  0.2448     0.7279 0.924 0.000 0.076
#&gt; GSM520701     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520702     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520703     3  0.6295     0.9919 0.472 0.000 0.528
#&gt; GSM520671     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520672     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520673     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520681     1  0.5882    -0.3244 0.652 0.000 0.348
#&gt; GSM520682     1  0.4062     0.5694 0.836 0.000 0.164
#&gt; GSM520680     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520677     1  0.6260    -0.7669 0.552 0.000 0.448
#&gt; GSM520678     1  0.4555     0.4718 0.800 0.000 0.200
#&gt; GSM520679     1  0.4178     0.5499 0.828 0.000 0.172
#&gt; GSM520674     1  0.3340     0.6510 0.880 0.000 0.120
#&gt; GSM520675     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520676     1  0.0892     0.7682 0.980 0.000 0.020
#&gt; GSM520686     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520687     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520688     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520683     1  0.0892     0.7682 0.980 0.000 0.020
#&gt; GSM520684     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520685     1  0.0000     0.7768 1.000 0.000 0.000
#&gt; GSM520708     1  0.6267    -0.7802 0.548 0.000 0.452
#&gt; GSM520706     1  0.5497     0.0212 0.708 0.000 0.292
#&gt; GSM520707     1  0.4654     0.4440 0.792 0.000 0.208
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2 p3    p4
#&gt; GSM520665     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520666     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520667     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520704     2  0.1305      0.985 0.004 0.960 NA 0.000
#&gt; GSM520705     2  0.1305      0.985 0.004 0.960 NA 0.000
#&gt; GSM520711     2  0.1305      0.985 0.004 0.960 NA 0.000
#&gt; GSM520692     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520693     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520694     2  0.0000      0.986 0.000 1.000 NA 0.000
#&gt; GSM520689     2  0.1118      0.986 0.000 0.964 NA 0.000
#&gt; GSM520690     2  0.1118      0.986 0.000 0.964 NA 0.000
#&gt; GSM520691     2  0.1118      0.986 0.000 0.964 NA 0.000
#&gt; GSM520668     1  0.6404      0.512 0.644 0.000 NA 0.220
#&gt; GSM520669     1  0.6364      0.520 0.652 0.000 NA 0.204
#&gt; GSM520670     1  0.6359      0.514 0.648 0.000 NA 0.220
#&gt; GSM520713     4  0.0188      0.765 0.004 0.000 NA 0.996
#&gt; GSM520714     4  0.0188      0.765 0.004 0.000 NA 0.996
#&gt; GSM520715     4  0.0188      0.765 0.004 0.000 NA 0.996
#&gt; GSM520695     4  0.0921      0.771 0.028 0.000 NA 0.972
#&gt; GSM520696     4  0.0921      0.771 0.028 0.000 NA 0.972
#&gt; GSM520697     4  0.1022      0.771 0.032 0.000 NA 0.968
#&gt; GSM520709     4  0.0921      0.771 0.028 0.000 NA 0.972
#&gt; GSM520710     4  0.1022      0.771 0.032 0.000 NA 0.968
#&gt; GSM520712     4  0.1022      0.771 0.032 0.000 NA 0.968
#&gt; GSM520698     4  0.4753      0.541 0.128 0.000 NA 0.788
#&gt; GSM520699     4  0.4869      0.528 0.132 0.000 NA 0.780
#&gt; GSM520700     1  0.6357      0.536 0.644 0.000 NA 0.232
#&gt; GSM520701     4  0.0000      0.763 0.000 0.000 NA 1.000
#&gt; GSM520702     4  0.0188      0.760 0.004 0.000 NA 0.996
#&gt; GSM520703     4  0.0000      0.763 0.000 0.000 NA 1.000
#&gt; GSM520671     1  0.4643      0.730 0.656 0.000 NA 0.344
#&gt; GSM520672     1  0.4134      0.739 0.740 0.000 NA 0.260
#&gt; GSM520673     1  0.4624      0.733 0.660 0.000 NA 0.340
#&gt; GSM520681     4  0.4008      0.559 0.244 0.000 NA 0.756
#&gt; GSM520682     4  0.4843      0.150 0.396 0.000 NA 0.604
#&gt; GSM520680     1  0.4331      0.755 0.712 0.000 NA 0.288
#&gt; GSM520677     4  0.2647      0.718 0.120 0.000 NA 0.880
#&gt; GSM520678     4  0.4605      0.377 0.336 0.000 NA 0.664
#&gt; GSM520679     4  0.4643      0.354 0.344 0.000 NA 0.656
#&gt; GSM520674     4  0.4855      0.139 0.400 0.000 NA 0.600
#&gt; GSM520675     1  0.4643      0.729 0.656 0.000 NA 0.344
#&gt; GSM520676     1  0.4972      0.475 0.544 0.000 NA 0.456
#&gt; GSM520686     1  0.4331      0.755 0.712 0.000 NA 0.288
#&gt; GSM520687     1  0.4454      0.754 0.692 0.000 NA 0.308
#&gt; GSM520688     1  0.4382      0.756 0.704 0.000 NA 0.296
#&gt; GSM520683     1  0.4955      0.513 0.556 0.000 NA 0.444
#&gt; GSM520684     1  0.4477      0.752 0.688 0.000 NA 0.312
#&gt; GSM520685     1  0.4746      0.692 0.632 0.000 NA 0.368
#&gt; GSM520708     4  0.2704      0.714 0.124 0.000 NA 0.876
#&gt; GSM520706     4  0.4431      0.456 0.304 0.000 NA 0.696
#&gt; GSM520707     4  0.4730      0.287 0.364 0.000 NA 0.636
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM520665     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520666     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520667     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520704     2  0.0609     0.9430 0.000 0.980 0.000 0.000 NA
#&gt; GSM520705     2  0.0955     0.9382 0.000 0.968 0.004 0.000 NA
#&gt; GSM520711     2  0.0865     0.9400 0.000 0.972 0.004 0.000 NA
#&gt; GSM520692     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520693     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520694     2  0.1965     0.9497 0.000 0.904 0.000 0.000 NA
#&gt; GSM520689     2  0.0000     0.9477 0.000 1.000 0.000 0.000 NA
#&gt; GSM520690     2  0.0290     0.9465 0.000 0.992 0.000 0.000 NA
#&gt; GSM520691     2  0.0290     0.9465 0.000 0.992 0.000 0.000 NA
#&gt; GSM520668     3  0.5394     0.9339 0.280 0.000 0.628 0.092 NA
#&gt; GSM520669     3  0.5409     0.9462 0.304 0.000 0.612 0.084 NA
#&gt; GSM520670     3  0.5440     0.9470 0.300 0.000 0.612 0.088 NA
#&gt; GSM520713     4  0.0898     0.7146 0.008 0.000 0.020 0.972 NA
#&gt; GSM520714     4  0.0693     0.7162 0.008 0.000 0.012 0.980 NA
#&gt; GSM520715     4  0.0798     0.7155 0.008 0.000 0.016 0.976 NA
#&gt; GSM520695     4  0.0404     0.7179 0.012 0.000 0.000 0.988 NA
#&gt; GSM520696     4  0.0290     0.7178 0.008 0.000 0.000 0.992 NA
#&gt; GSM520697     4  0.0290     0.7178 0.008 0.000 0.000 0.992 NA
#&gt; GSM520709     4  0.0771     0.7166 0.020 0.000 0.004 0.976 NA
#&gt; GSM520710     4  0.0609     0.7168 0.020 0.000 0.000 0.980 NA
#&gt; GSM520712     4  0.0609     0.7168 0.020 0.000 0.000 0.980 NA
#&gt; GSM520698     4  0.5274     0.1262 0.064 0.000 0.336 0.600 NA
#&gt; GSM520699     4  0.5188     0.1098 0.056 0.000 0.344 0.600 NA
#&gt; GSM520700     3  0.5900     0.8575 0.376 0.000 0.516 0.108 NA
#&gt; GSM520701     4  0.1018     0.7058 0.016 0.000 0.016 0.968 NA
#&gt; GSM520702     4  0.1018     0.7058 0.016 0.000 0.016 0.968 NA
#&gt; GSM520703     4  0.0912     0.7073 0.012 0.000 0.016 0.972 NA
#&gt; GSM520671     1  0.3730     0.8833 0.712 0.000 0.000 0.288 NA
#&gt; GSM520672     1  0.4199     0.7744 0.764 0.000 0.056 0.180 NA
#&gt; GSM520673     1  0.3684     0.8862 0.720 0.000 0.000 0.280 NA
#&gt; GSM520681     4  0.3756     0.4813 0.248 0.000 0.008 0.744 NA
#&gt; GSM520682     4  0.4504    -0.1040 0.428 0.000 0.008 0.564 NA
#&gt; GSM520680     1  0.3395     0.8833 0.764 0.000 0.000 0.236 NA
#&gt; GSM520677     4  0.2280     0.6465 0.120 0.000 0.000 0.880 NA
#&gt; GSM520678     4  0.4171     0.0842 0.396 0.000 0.000 0.604 NA
#&gt; GSM520679     4  0.4341     0.0433 0.404 0.000 0.004 0.592 NA
#&gt; GSM520674     4  0.4546    -0.2229 0.460 0.000 0.008 0.532 NA
#&gt; GSM520675     1  0.3752     0.8754 0.708 0.000 0.000 0.292 NA
#&gt; GSM520676     1  0.4499     0.6400 0.584 0.000 0.004 0.408 NA
#&gt; GSM520686     1  0.3519     0.8576 0.776 0.000 0.008 0.216 NA
#&gt; GSM520687     1  0.3550     0.8811 0.760 0.000 0.004 0.236 NA
#&gt; GSM520688     1  0.3521     0.8832 0.764 0.000 0.004 0.232 NA
#&gt; GSM520683     1  0.4375     0.7520 0.628 0.000 0.004 0.364 NA
#&gt; GSM520684     1  0.3715     0.8898 0.736 0.000 0.004 0.260 NA
#&gt; GSM520685     1  0.4047     0.8525 0.676 0.000 0.004 0.320 NA
#&gt; GSM520708     4  0.3336     0.5258 0.228 0.000 0.000 0.772 NA
#&gt; GSM520706     4  0.4397    -0.0419 0.432 0.000 0.004 0.564 NA
#&gt; GSM520707     4  0.4443    -0.2219 0.472 0.000 0.004 0.524 NA
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM520665     2  0.2416    0.91087 0.000 0.844 0.000 0.000 NA 0.000
#&gt; GSM520666     2  0.2416    0.91087 0.000 0.844 0.000 0.000 NA 0.000
#&gt; GSM520667     2  0.2416    0.91087 0.000 0.844 0.000 0.000 NA 0.000
#&gt; GSM520704     2  0.1152    0.89073 0.000 0.952 0.000 0.000 NA 0.044
#&gt; GSM520705     2  0.1493    0.88314 0.000 0.936 0.004 0.000 NA 0.056
#&gt; GSM520711     2  0.1493    0.88314 0.000 0.936 0.004 0.000 NA 0.056
#&gt; GSM520692     2  0.2378    0.91193 0.000 0.848 0.000 0.000 NA 0.000
#&gt; GSM520693     2  0.2378    0.91193 0.000 0.848 0.000 0.000 NA 0.000
#&gt; GSM520694     2  0.2378    0.91193 0.000 0.848 0.000 0.000 NA 0.000
#&gt; GSM520689     2  0.0146    0.90626 0.000 0.996 0.000 0.000 NA 0.000
#&gt; GSM520690     2  0.0146    0.90626 0.000 0.996 0.000 0.000 NA 0.000
#&gt; GSM520691     2  0.0146    0.90626 0.000 0.996 0.000 0.000 NA 0.000
#&gt; GSM520668     3  0.1585    0.93277 0.036 0.000 0.940 0.012 NA 0.012
#&gt; GSM520669     3  0.1726    0.93664 0.044 0.000 0.932 0.012 NA 0.012
#&gt; GSM520670     3  0.1367    0.93682 0.044 0.000 0.944 0.012 NA 0.000
#&gt; GSM520713     4  0.2468    0.85798 0.096 0.000 0.016 0.880 NA 0.008
#&gt; GSM520714     4  0.2466    0.85605 0.112 0.000 0.008 0.872 NA 0.008
#&gt; GSM520715     4  0.2466    0.85605 0.112 0.000 0.008 0.872 NA 0.008
#&gt; GSM520695     4  0.2020    0.86073 0.096 0.000 0.000 0.896 NA 0.008
#&gt; GSM520696     4  0.1970    0.86000 0.092 0.000 0.000 0.900 NA 0.008
#&gt; GSM520697     4  0.1908    0.86117 0.096 0.000 0.000 0.900 NA 0.004
#&gt; GSM520709     4  0.2218    0.85969 0.104 0.000 0.000 0.884 NA 0.012
#&gt; GSM520710     4  0.2266    0.85839 0.108 0.000 0.000 0.880 NA 0.012
#&gt; GSM520712     4  0.2165    0.85946 0.108 0.000 0.000 0.884 NA 0.008
#&gt; GSM520698     4  0.5667    0.28930 0.052 0.000 0.352 0.540 NA 0.056
#&gt; GSM520699     4  0.5603    0.24368 0.044 0.000 0.372 0.528 NA 0.056
#&gt; GSM520700     3  0.4414    0.82204 0.152 0.000 0.752 0.056 NA 0.040
#&gt; GSM520701     4  0.2889    0.83926 0.096 0.000 0.004 0.856 NA 0.044
#&gt; GSM520702     4  0.2981    0.83698 0.100 0.000 0.008 0.852 NA 0.040
#&gt; GSM520703     4  0.2752    0.84331 0.096 0.000 0.004 0.864 NA 0.036
#&gt; GSM520671     1  0.0458    0.81189 0.984 0.000 0.000 0.016 NA 0.000
#&gt; GSM520672     1  0.1644    0.75499 0.932 0.000 0.052 0.004 NA 0.012
#&gt; GSM520673     1  0.0717    0.81190 0.976 0.000 0.000 0.016 NA 0.008
#&gt; GSM520681     1  0.4887    0.00878 0.476 0.000 0.004 0.472 NA 0.048
#&gt; GSM520682     1  0.4246    0.62574 0.692 0.000 0.012 0.268 NA 0.028
#&gt; GSM520680     1  0.0603    0.79475 0.980 0.000 0.016 0.000 NA 0.004
#&gt; GSM520677     4  0.3711    0.63433 0.260 0.000 0.000 0.720 NA 0.020
#&gt; GSM520678     1  0.4196    0.55661 0.640 0.000 0.000 0.332 NA 0.028
#&gt; GSM520679     1  0.4150    0.58383 0.652 0.000 0.000 0.320 NA 0.028
#&gt; GSM520674     1  0.4072    0.66674 0.704 0.000 0.004 0.260 NA 0.032
#&gt; GSM520675     1  0.0862    0.81240 0.972 0.000 0.004 0.016 NA 0.008
#&gt; GSM520676     1  0.3014    0.77388 0.832 0.000 0.000 0.132 NA 0.036
#&gt; GSM520686     1  0.0508    0.79855 0.984 0.000 0.012 0.000 NA 0.004
#&gt; GSM520687     1  0.0405    0.80812 0.988 0.000 0.004 0.008 NA 0.000
#&gt; GSM520688     1  0.0146    0.80295 0.996 0.000 0.004 0.000 NA 0.000
#&gt; GSM520683     1  0.2046    0.80614 0.908 0.000 0.000 0.060 NA 0.032
#&gt; GSM520684     1  0.0767    0.80671 0.976 0.000 0.012 0.008 NA 0.004
#&gt; GSM520685     1  0.1523    0.81182 0.940 0.000 0.008 0.044 NA 0.008
#&gt; GSM520708     4  0.4672    0.40124 0.348 0.000 0.000 0.596 NA 0.056
#&gt; GSM520706     1  0.4047    0.60909 0.676 0.000 0.000 0.296 NA 0.028
#&gt; GSM520707     1  0.3841    0.66535 0.716 0.000 0.000 0.256 NA 0.028
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<pre><code>#&gt; Error in mat[ceiling(1:nr/h_ratio), ceiling(1:nc/w_ratio), drop = FALSE]: subscript out of bounds
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n cell.type(p) cell.line(p) other(p) k
#> ATC:NMF 51     1.46e-11     9.31e-07 5.89e-03 2
#> ATC:NMF 45     1.69e-10     8.37e-09 2.79e-06 3
#> ATC:NMF 44     2.79e-10     5.86e-07 1.08e-04 4
#> ATC:NMF 42     4.01e-09     6.41e-09 3.52e-08 5
#> ATC:NMF 47     3.48e-10     1.21e-11 1.38e-09 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


