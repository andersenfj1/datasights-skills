# Widget settings reference

Widget appearance and behavior live in the widget's `widget_settings` JSON, organized into settings groups. Each widget kind accepts a fixed set of groups, and each group has a fixed set of patchable keys. Enum-valued keys list their exact accepted values inline; anything not listed here is not a valid settings key.

## Widget kinds

| Widget kind | Settings groups | Chart field roles |
| --- | --- | --- |
| `line` | `widgetDisplay`, `line` | `xField`, `yField` |
| `bar` | `widgetDisplay`, `line`, `bar` | `categoryField`, `valueField`, `seriesField` |
| `combo` | `widgetDisplay`, `cartesianChart`, `line`, `bar` | `categoryField`, `valueField`, `seriesField`, `xField`, `yField` |
| `pie` | `widgetDisplay`, `line`, `pie` | `categoryField`, `valueField` |
| `funnel` | `widgetDisplay`, `line`, `funnel` | `stageField`, `valueField` |
| `radar` | `widgetDisplay`, `line`, `radar` | `categoryField`, `valueField` |
| `sankey` | `widgetDisplay`, `line`, `sankey` | `sourceField`, `targetField`, `valueField` |
| `kpi` | `widgetDisplay`, `kpi` | (none) |
| `dataGrid` | `widgetDisplay`, `dataGrid` | (none) |
| `pivotTable` | `widgetDisplay`, `pivotTable` | (none) |
| `maps` | `widgetDisplay`, `maps` | (none) |
| `summary` | `widgetDisplay`, `summary` | (none) |
| `customHtml` | `widgetDisplay`, `customHtml` | (none) |
| `freeText` | `widgetDisplay`, `freeText` | (none) |
| `unknown` | `widgetDisplay` | (none) |

## Font family values

Font-family keys (`titleFontFamily`, `legendFontFamily`, `pointLabelFontFamily`, `xAxisNameFontFamily`, `xAxisLabelFontFamily`, `yAxisNameFontFamily`, `yAxisLabelFontFamily`) store the full CSS font stack string, not a display label. Exact accepted values (the empty string means the theme default font):

- `""`
- `"\"Calibri\", \"Carlito\", \"Segoe UI\", sans-serif"`
- `"\"Space Grotesk\", \"Inter\", system-ui, sans-serif"`
- `"\"IBM Plex Sans\", \"Inter\", system-ui, sans-serif"`
- `"\"Merriweather\", Georgia, serif"`
- `"\"Source Code Pro\", \"SFMono-Regular\", Menlo, monospace"`

## Draft actions

| Action | What it does |
| --- | --- |
| `set_model` | Changes the selected model draft; use only when the user asks to use a different model. |
| `replace_query_config` | Edits the query-builder draft. It changes available output data, not visual styling. |
| `update_chart_fields` | Maps existing query output columns onto widget visual roles. |
| `update_chart_settings` | Edits settings-pane controls under payload.settings for the selected widget kind. |
| `edit_custom_html_template` | Edits the Custom HTML template with exact before/after patch hunks; use for targeted template edits. |
| `set_widget_type` | Switches the widget draft type or chart mode without saving. |
| `update_display` | Toggles payload.showWidgetName only. |

## `line` widgets

Line chart widget drafts only. Help choose the smallest valid draft action set for line-chart requests.

### Chart field roles

- `xField` - Horizontal axis or time/category field; must be a query output column.
- `yField` - Vertical numeric/value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for line style, title, legend, colors, point labels, x/y axes, swap axes, zoom, total line, tooltip, field formats, and multi-series settings.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what data to fetch, select, aggregate, group, filter, sort, or limit.
2. Use update_chart_fields when the user asks which returned query output columns should drive the line chart.
3. Use update_chart_settings when the user asks for visual behavior or settings-pane controls such as title, legend, axes, labels, colors, zoom, total line, format, or multi-chart.
4. Use set_widget_type with chart_mode only when the user asks to switch chart type or chart mode (bar/line/combo).
5. Use update_display only for the widget name visibility toggle.
6. When a field should appear on an axis but is not in the current query output, include a query action that exposes it before the chart-field action.
7. When the user asks for different colors, a palette name, or a color mood (lighter, muted, reddish), set line.colorPaletteId to the best-matching id from the Available color palettes context list.
8. When the user asks to change how axis values are displayed (dates, decimals, separators), use line.xFormat/yFormat with the related decimals and separator keys, not label rotation or axis names.

### Examples

- "Group by month and show revenue over time." -> `replace_query_config` + `update_chart_fields`. The query must expose grouped month/revenue output, then the chart fields map those outputs to xField/yField.
- "Turn on the total line." -> `update_chart_settings`. Total line is a line settings-pane control, not a query or chart-field change.
- "Make the x-axis show short dates." -> `update_chart_settings`. Axis value display is line.xFormat (e.g. "short_date"), a settings-pane control.

## `bar` widgets

Bar chart widget drafts only. Help choose the smallest valid draft action set for bar-chart requests.

### Chart field roles

- `categoryField` - Bar category/grouping field shown along the category axis; must be a query output column.
- `valueField` - Numeric bar value/measure field; must be a query output column.
- `seriesField` - Optional split/series field for grouped bars; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `bar` - Use for bar-specific controls: series mode, multi-series fields, orientation, multi-chart, stacked, maxWidth, and borderRadius.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what data to fetch, select, aggregate, group, filter, sort, or limit.
2. Use update_chart_fields when the user asks which returned query output columns should drive category, value, or series roles.
3. Use update_chart_settings when the user asks for visual behavior or settings-pane controls such as title, legend, axes, labels, colors, stacked bars, orientation, max width, border radius, or multi-chart.
4. When the user asks to swap the axes, flip the chart, or make bars horizontal, use update_chart_settings with bar.orientation="horizontal" (or "vertical" to restore); do not use set_widget_type or update_chart_fields for that.
5. When the user asks for different colors, a palette name, or a color mood (lighter, muted, reddish), set line.colorPaletteId to the best-matching id from the Available color palettes context list.
6. Use set_widget_type with chart_mode only when the user asks to switch chart type or chart mode (bar/line/combo).
7. Use update_display only for the widget name visibility toggle.
8. When category/value/series fields are not in the current query output, include a query action that exposes them before the chart-field action.

### Examples

- "Stack the bars." -> `update_chart_settings`. Stacked is a bar settings-pane control, not a query or chart-field change.
- "Swap the axes." -> `update_chart_settings`. On a bar chart, swapping axes means bar.orientation="horizontal", a settings-pane control.
- "Show total emissions by subsidiary as a bar chart." -> `set_widget_type` + `replace_query_config` + `update_chart_fields`. The chart type changes, the query must expose subsidiary and aggregated emissions, then bar fields map category/value.

## `combo` widgets

Combo chart widget drafts only. Help choose the smallest valid draft action set for mixed bar/line chart requests.

### Chart field roles

- `categoryField` - Category/grouping field for bar-style combo output; must be a query output column.
- `valueField` - Numeric value field for bar-style combo output; must be a query output column.
- `seriesField` - Optional split/series field for grouped combo output; must be a query output column.
- `xField` - Horizontal axis or time/category field; must be a query output column.
- `yField` - Vertical numeric/value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `cartesianChart` - Use for combo-specific mode, seriesTypes, seriesAxes, and secondaryYAxis controls.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `bar` - Use for bar-specific controls within the combo workbench: series mode, orientation, multi-chart, stacked, maxWidth, and borderRadius.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what data to fetch, select, aggregate, group, filter, sort, or limit.
2. Use update_chart_fields when the user asks which returned query output columns should drive category/value/series or x/y roles.
3. Use update_chart_settings.cartesianChart for combo mode, per-series bar/line rendering, per-series left/right axis assignment, or secondary y-axis options.
4. Use update_chart_settings.line for shared title, legend, colors, axes, labels, tooltip, and formatting controls.
5. Use update_chart_settings.bar for bar-specific orientation, stacking, bar width, border radius, or bar multi-series controls.
6. Use set_widget_type only when the user asks to switch widget type or chart mode.
7. When a requested field is not in the current query output, include a query action that exposes it before mapping fields or series settings.

### Examples

- "Make revenue a line on the right axis and costs bars." -> `update_chart_settings`. Per-series rendering and right-axis assignment are combo cartesian settings, not query changes if the fields already exist.
- "Show monthly revenue and cost as a combo chart." -> `set_widget_type` + `replace_query_config` + `update_chart_fields`. The type changes, the query must expose month/revenue/cost, then fields map those outputs into the combo chart.

## `pie` widgets

Pie chart widget drafts only. Help choose the smallest valid draft action set for pie and donut chart requests.

### Chart field roles

- `categoryField` - Pie slice/category field; must be a query output column.
- `valueField` - Numeric slice value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `pie` - Use for pie-specific controls: style, labelType, sliceSort, outerLabelAlign, topNLimit, topNOtherLabel, sliceSeparatorWidth, percentLabelDecimals, and multiChart.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks which slices or values the pie should use, including grouping and aggregation.
2. Use update_chart_fields when the user asks which returned query output columns should drive category and value roles.
3. Use update_chart_settings.pie for style, labels, sorting, top-N grouping, slice separator, percent decimals, or pie multi-chart controls.
4. Use update_chart_settings.line for shared title, legend, colors, tooltip, labels, and formatting controls.
5. Use set_widget_type only when the user asks to switch widget type.
6. When category/value fields are not in the current query output, include a query action that exposes them before the chart-field action.

### Examples

- "Show the top five categories and group the rest as Other." -> `update_chart_settings`. Top-N grouping and the other-slice label are pie settings controls.
- "Show spend share by supplier." -> `replace_query_config` + `update_chart_fields`. The query must expose supplier and aggregated spend, then category/value map to pie fields.

## `funnel` widgets

Funnel chart widget drafts only. Help choose the smallest valid draft action set for funnel chart requests.

### Chart field roles

- `stageField` - Funnel stage/category field; must be a query output column.
- `valueField` - Numeric funnel stage value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `funnel` - Use for funnel-specific controls: stageField, valueField, labelMode, and labelColor.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks which stages or values the funnel should use, including grouping and aggregation.
2. Use update_chart_fields when the user asks which returned query output columns should drive stage and value roles.
3. Use update_chart_settings.funnel for stageField, valueField, labelMode, or labelColor.
4. Use update_chart_settings.line for shared title, legend, colors, tooltip, labels, and formatting controls.
5. Use set_widget_type only when the user asks to switch widget type.
6. When stage/value fields are not in the current query output, include a query action that exposes them before field or settings changes.

### Examples

- "Use pipeline_stage as the stage and count as the value." -> `update_chart_fields`. The user is mapping existing output columns to funnel visual roles.
- "Make the funnel labels black." -> `update_chart_settings`. Label color is a funnel settings-pane control.

## `radar` widgets

Radar chart widget drafts only. Help choose the smallest valid draft action set for radar chart requests.

### Chart field roles

- `categoryField` - Radar axis/dimension field; must be a query output column.
- `valueField` - Numeric radar value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `radar` - Use for radar-specific controls: shape, areaOpacity, splitNumber, axisLabelShow, and multiChart.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what dimensions or measures the radar should compare.
2. Use update_chart_fields when the user asks which returned query output columns should drive category and value roles.
3. Use update_chart_settings.radar for shape, area opacity, split count, axis labels, or radar multi-chart controls.
4. Use update_chart_settings.line for shared title, legend, colors, tooltip, labels, and formatting controls.
5. Use set_widget_type only when the user asks to switch widget type.
6. When category/value fields are not in the current query output, include a query action that exposes them before field or settings changes.

### Examples

- "Make the radar circular with fewer rings." -> `update_chart_settings`. Shape and split count are radar settings-pane controls.
- "Compare emissions across categories on a radar chart." -> `set_widget_type` + `replace_query_config` + `update_chart_fields`. The widget type changes, the query must expose category and emissions, then radar fields map those outputs.

## `sankey` widgets

Sankey chart widget drafts only. Help choose the smallest valid draft action set for sankey flow requests.

### Chart field roles

- `sourceField` - Sankey source node field; must be a query output column.
- `targetField` - Sankey target node field; must be a query output column.
- `valueField` - Numeric sankey link weight/value field; must be a query output column.

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `line` - Use for shared chart chrome: title, legend, colors, point labels, axes, zoom, total line, tooltips, field formats, and multi-series field formatting.
- `sankey` - Use for sankey-specific controls: sourceField, targetField, valueField, nodeWidth, nodeGap, nodeAlign, linkColorMode, linkOpacity, curveness, draggable, showNodeLabels, and labelColor.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what source, target, or flow value data the sankey should use.
2. Use update_chart_fields when the user asks which returned query output columns should drive source, target, and value roles.
3. Use update_chart_settings.sankey for sourceField, targetField, valueField, node width/gap/alignment, link color/opacity/curveness, draggable nodes, node labels, or label color.
4. Use update_chart_settings.line for shared title, legend, colors, tooltip, labels, and formatting controls.
5. Use set_widget_type only when the user asks to switch widget type.
6. When source/target/value fields are not in the current query output, include a query action that exposes them before field or settings changes.

### Examples

- "Make links more transparent." -> `update_chart_settings`. Link opacity is a sankey settings-pane control.
- "Show flows from origin to destination by total volume." -> `replace_query_config` + `update_chart_fields`. The query must expose origin, destination, and volume, then sankey fields map those outputs.

## `kpi` widgets

KPI widget drafts only. Help choose the smallest valid draft action set for KPI metric requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `kpi` - Use for KPI-specific controls: alignment, headerColor, headerSize, headerWeight, headerStyle, valueColor, valueSize, valueWeight, and valueStyle.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks what metric the KPI should calculate, filter, aggregate, sort, or limit.
2. Do not use update_chart_fields for KPI widgets; KPI display is driven by query output and KPI settings.
3. Use update_chart_settings.kpi for alignment, header styling, or value styling.
4. Use set_widget_type only when the user asks to switch widget type.
5. Use update_display only for the widget name visibility toggle.
6. When the KPI needs a different metric than the query currently returns, change the query rather than inventing a settings key.

### Examples

- "Make the KPI value bigger and blue." -> `update_chart_settings`. Value size and color are KPI settings-pane controls.
- "Show total revenue as a KPI." -> `replace_query_config`. The metric itself comes from the query output, not a KPI style setting.

## `dataGrid` widgets

Data grid widget drafts only. Help choose the smallest valid draft action set for tabular grid requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `dataGrid` - Use for data-grid-specific controls: pageSize, density, zebraRows, stickyHeader, allowColumnSort, showRowRange, truncateCells, defaultSort, columns, editable, and conditionalFormatting.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the user asks which rows or columns the grid should fetch, filter, sort, group, or limit.
2. Do not use update_chart_fields for data grid widgets; the displayed data comes from query output.
3. Use update_chart_settings.dataGrid for page size, density, row styling, sticky headers, column sorting, row range, truncation, default sort, column labels/visibility/widths, editable settings, or conditional formatting.
4. For conditional formatting, use dataGrid.conditionalFormatting.rules with controlled style fields only: backgroundColor, textColor, bold, italic, and borderColor. Do not use arbitrary CSS.
5. Conditional-formatting sourceField and target column fields must be existing query output fields. If the requested field is not exposed by the query but exists in selected model fields, include a query action to expose it.
6. Use condition rules for comparisons such as equals, contains, greater/less, between, blank, and not blank. Use colorScale rules for numeric green-to-red or red-to-green gradients.
7. Use query sorting when the user wants the fetched result ordered; use dataGrid.defaultSort when the user asks for the grid's initial UI sort.
8. Use set_widget_type only when the user asks to switch widget type.
9. When a configured column is not in the current query output, include a query action that exposes it before the grid settings action.

### Examples

- "Hide the internal id column and rename customer_name." -> `update_chart_settings`. Column visibility and labels are dataGrid.columns settings controls.
- "Only show rows from 2025 sorted by revenue descending." -> `replace_query_config`. Filtering and fetched-result ordering belong in the query builder.
- "Make rows red where status is delayed." -> `update_chart_settings`. Row highlighting belongs in dataGrid.conditionalFormatting using a condition rule.
- "Highlight revenue cells above 100000." -> `update_chart_settings`. Cell highlighting belongs in dataGrid.conditionalFormatting using a condition rule.
- "Add a green-to-red scale for margin." -> `update_chart_settings`. Numeric gradients belong in dataGrid.conditionalFormatting using a colorScale rule.
- "Color the customer column based on risk score." -> `update_chart_settings`. A condition can read one source field and apply formatting to a different target column.

## `pivotTable` widgets

Pivot table widget drafts only. Help choose the smallest valid draft action set for pivot-table requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `pivotTable` - Use for pivot-specific controls: rows, columns, measures, subtotals, grand totals, nullValueDisplay, clickFiltering, valueSort, valueFilters, defaultExpansion, rowHeaderWidthPx, defaultValueColumnWidthPx, and valueColumnWidths.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the pivot needs fields that are not currently returned by the query.
2. Do not use update_chart_fields for pivot tables; pivot rows, columns, and measures are settings-pane controls.
3. Use update_chart_settings.pivotTable for rows, columns, measures, subtotals, grand totals, null display, click filtering, value sorting/filtering, expansion, and widths.
4. Use pivotTable.measures for aggregate choices rather than query aggregation when the user is configuring pivot measures.
5. Use set_widget_type only when the user asks to switch widget type.
6. When a row, column, or measure field is not in the current query output, include a query action that exposes it before the pivot settings action.

### Examples

- "Put subsidiary on rows, month on columns, and sum emissions." -> `update_chart_settings`. Pivot dimensions and measures are pivotTable settings controls if those fields are already available.
- "Add country to the pivot rows but it is not in the output." -> `replace_query_config` + `update_chart_settings`. The query must expose country before pivotTable.rows can use it.

## `maps` widgets

Map widget drafts only. Help choose the smallest valid draft action set for map widget requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `maps` - Use for maps-specific controls: map center/zoom/type and layers for markers, heatmaps, polygons, routes, labels, tooltips, styles, and cross-filtering.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use replace_query_config when the map needs different latitude, longitude, geometry, label, weight, color, size, tooltip, or cross-filter fields.
2. Do not use update_chart_fields for map widgets; map layer field bindings are settings-pane controls.
3. Use update_chart_settings.maps.map for base map center, zoom, and map type controls.
4. Use update_chart_settings.maps.layers for marker, heatmap, polygon, route, tooltip, styling, and cross-filter layer controls.
5. Use set_widget_type only when the user asks to switch widget type.
6. When a layer references a field that is not in the current query output, include a query action that exposes it before the maps settings action.

### Examples

- "Use latitude and longitude columns for blue clustered pins." -> `update_chart_settings`. Lat/lng bindings and marker styling are map layer settings if the fields already exist.
- "Make this a heatmap weighted by incidents." -> `replace_query_config` + `update_chart_settings`. The query may need to expose incidents, then the map layer switches to heatmap and binds the weight field.

## `summary` widgets

Summary widget drafts only. Help choose the smallest valid draft action set for summary widget requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `summary` - Use for summary-specific controls: mode, contextPrompt, referenceWidgetId, referenceBoardId, and referencePanelId.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use update_chart_settings.summary for summary mode, prompt/context text, reference widget, reference board, or reference panel controls.
2. Use replace_query_config only when the summary is explicitly based on the selected model query output and the user asks to change that data.
3. Do not use update_chart_fields for summary widgets.
4. Use set_widget_type only when the user asks to switch widget type.
5. Use update_display only for the widget name visibility toggle.
6. If the user asks to generate or refresh summary text rather than configure the widget draft, explain that only draft settings can be changed here.

### Examples

- "Summarize the sales chart and focus on outliers." -> `update_chart_settings`. The referenced widget and context prompt are summary settings-pane controls.
- "Hide the summary widget name." -> `update_display`. Widget name visibility is a display toggle.

## `customHtml` widgets

Custom HTML widget drafts only. Help choose the smallest valid draft action set for custom HTML widget requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `customHtml` - Use for the customHtml.template Handlebars HTML template only.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use edit_custom_html_template for targeted edits to an existing Custom HTML template.
2. Use update_chart_settings.customHtml.template only when creating a new template or when the user explicitly asks to replace or rebuild the entire template.
3. If the current widget is Custom HTML and the user asks to build a chart, card, table, KPI, or other visual, build it in customHtml.template with HTML/CSS/JavaScript/Handlebars instead of switching widget type.
4. For targeted edits to an existing customHtml.template, preserve unrelated HTML, CSS, colors, backgrounds, borders, layout, scripts, Handlebars loops, and earlier requested behavior; do not regenerate or restyle the full template.
5. For number/date formatting requests, change only the formatting expressions, helpers, or scripts needed for that formatting; do not change cell backgrounds, colors, layout, or other styling unless explicitly asked.
6. If the provided Custom HTML context is not enough for a safe targeted edit, ask for confirmation instead of guessing or recreating the template, and do not explain internal context availability or limits.
7. Generated styling must use inline style attributes; do not create custom CSS classes, class attributes, or class selectors for styling. Use ids or data attributes instead of classes when JavaScript needs element hooks.
8. Generated Custom HTML and JavaScript must be readable multi-line code with stable indentation; never return minified or one-line templates, style blocks, script blocks, Handlebars loops, tables, or chart markup.
9. When building a new template or explicitly replacing one from scratch and colors are not explicitly requested, write theme-aware readable colors into the generated customHtml.template CSS by using the iframe CSS variables --widget-text, --widget-muted-text, --widget-surface, --widget-border, and --widget-accent; the renderer exposes these variables but does not enforce them globally.
10. Use replace_query_config when the template needs different row, rows, columns, or data fields exposed to it.
11. Do not use update_chart_fields for custom HTML widgets.
12. JavaScript <script> tags are allowed in customHtml.template and run inside the widget's sandboxed iframe.
13. Use set_widget_type only when the user explicitly asks to switch or change the widget type.
14. Use update_display only for the widget name visibility toggle.

### Examples

- "Format the total column with thousands separators." -> `edit_custom_html_template`. This is a targeted template code edit, so only the relevant exact before/after snippet should be changed.
- "Make the custom HTML show the first row title and value." -> `update_chart_settings`. The rendered markup is controlled by customHtml.template.
- "Build a bar chart of emissions by month." -> `replace_query_config` + `update_chart_settings`. The query must expose the data fields, then customHtml.template renders the chart as HTML/CSS/Handlebars without changing widget type.

## `freeText` widgets

Free text widget drafts only. Help choose the smallest valid draft action set for free text widget requests.

### Chart field roles

_This widget kind does not use `update_chart_fields`._

### Settings groups

- `widgetDisplay` - Use only for showWidgetName.
- `freeText` - Use for free-text-specific controls: content, fontSize, fontFamily, alignment, and color.

Key-by-key details for every group are in the [Settings group key reference](#settings-group-key-reference).

### Decision rules

1. Use update_chart_settings.freeText for text content, font size, font family, alignment, or color changes.
2. Do not use replace_query_config for free text unless the user explicitly asks to switch to a data-backed widget.
3. Do not use update_chart_fields for free text widgets.
4. Use set_widget_type only when the user asks to switch widget type.
5. Use update_display only for the widget name visibility toggle.
6. Keep HTML content within the sanitized free-text capabilities; do not add scripts or unsafe embeds.

### Examples

- "Center the note and make it larger." -> `update_chart_settings`. Alignment and font size are freeText settings-pane controls.
- "Replace the text with a short explanation." -> `update_chart_settings`. Free-text content is controlled by freeText.content.

## Settings group key reference

Every patchable `widget_settings` key by settings group. Enum-valued keys list their exact accepted values inline; font-family keys accept the values in [Font family values](#font-family-values).

### `widgetDisplay` group

Used by widget kinds: `line`, `bar`, `combo`, `pie`, `funnel`, `radar`, `sankey`, `kpi`, `dataGrid`, `pivotTable`, `maps`, `summary`, `customHtml`, `freeText`, `unknown`

| Key | Description |
| --- | --- |
| `showWidgetName` | Shows or hides the widget name/header in the rendered widget. |

### `cartesianChart` group

Used by widget kinds: `combo`

| Key | Description |
| --- | --- |
| `mode` | Sets the cartesian chart draft mode for combo-capable charts. Valid values: "bar", "line", "combo". For an explicit chart-type switch request, prefer set_widget_type with chart_mode; both reach the same draft mode. |
| `seriesTypes` | Maps individual series to bar or line rendering in a combo chart, as an object like {"revenue": "line", "costs": "bar"}. Keys must be series names from the query output or legend. Valid values: "bar", "line". |
| `seriesAxes` | Maps individual series to the left or right axis in a combo chart, as an object like {"revenue": "right"}. Keys must be series names from the query output or legend. Valid values: "left", "right". |
| `secondaryYAxis` | Configures the combo chart right axis as an object: { show, name, min, max, format, decimals, separator }. Only useful when at least one series is assigned to the right axis via seriesAxes. |

### `line` group

Used by widget kinds: `line`, `bar`, `combo`, `pie`, `funnel`, `radar`, `sankey`

| Key | Description |
| --- | --- |
| `style` | Controls line rendering style. Valid values: "basic", "smooth", "area", "stacked_area", "stacked_line", "multi_series". Use "multi_series" together with multiSeriesFields. |
| `titleMode` | Controls whether the chart title is automatic, custom, or hidden. Valid values: "none", "custom", "group". |
| `titleText` | Sets one custom chart title. |
| `titleTexts` | Sets custom title text per chart or series where supported. |
| `titleColor` | Sets chart title text color. |
| `titleFontFamily` | Sets chart title font family. See the font family values rule for accepted values. |
| `titleFontStyle` | Sets chart title font style. Valid values: "normal", "italic". |
| `titleFontWeight` | Sets chart title font weight. Valid values: "normal", "500", "600", "700", "800". |
| `titlePlacement` | Sets where the chart title is placed. Valid values: "top_left", "top_center", "top_right", "bottom_left", "bottom_center", "bottom_right". |
| `titleSize` | Sets chart title font size. |
| `legendEnabled` | Shows or hides the chart legend. |
| `legendPlacement` | Sets where the legend is placed. Valid values: "top_left", "top_center", "top_right", "middle_left", "middle_right", "bottom_left", "bottom_center", "bottom_right". |
| `legendIcon` | Sets the legend marker icon shape. Valid values: "circle", "rect", "roundRect", "triangle", "diamond", "pin", "arrow", "none". |
| `legendSize` | Sets legend label font size. |
| `legendFontFamily` | Sets legend label font family. See the font family values rule for accepted values. |
| `legendAliases` | Maps raw legend keys to display labels without changing query fields. |
| `colorPaletteId` | Selects the chart color palette by numeric id. The id must be one of the ids listed under Available color palettes in the context; pick by palette_name or by matching the listed palette_colors to the user's description. |
| `colorPaletteStrategy` | Controls how palette colors are extended when there are more series than palette colors. Valid values: "cycle", "tint_shade", "hue_shift", "hash_series". |
| `colorSlotAssignments` | Pins specific fields, categories, or series to color palette slots, as an object mapping the series/category name to a zero-based palette slot index, e.g. {"Revenue": 0, "Costs": 3}. |
| `showPointLabels` | Shows or hides labels on chart points or marks. |
| `pointLabelPosition` | Sets where point labels are placed. Valid values: "top", "bottom", "left", "right", "inside", "insideTop", "insideBottom", "insideLeft", "insideRight". |
| `pointLabelFormat` | Adds prefix/suffix formatting to point labels. Valid values: "none", "prefix", "suffix". |
| `pointLabelContent` | Sets whether point labels show values, group labels, or both. Valid values: "value", "group", "group_value". |
| `pointLabelPrefix` | Adds text before point label values. |
| `pointLabelSuffix` | Adds text after point label values. |
| `pointLabelColor` | Sets point label text color. |
| `pointLabelSize` | Sets point label font size. |
| `pointLabelFontFamily` | Sets point label font family. See the font family values rule for accepted values. |
| `pointLabelHideOverlap` | Hides overlapping point labels when enabled. |
| `pointLabelSeries` | Limits point labels to specific series. |
| `enableZoom` | Enables or disables chart zoom controls. |
| `xAxisPosition` | Sets x-axis position. Valid values: "bottom", "top". |
| `xAxisType` | Sets the x-axis scale type. Valid values: "auto", "category", "value", "time", "log". Use "time" for date/time data, "category" for discrete labels. |
| `xAxisName` | Sets x-axis title text. |
| `xAxisNameColor` | Sets x-axis title color. |
| `xAxisNameFontFamily` | Sets x-axis title font family. See the font family values rule for accepted values. |
| `xAxisNameSize` | Sets x-axis title font size. |
| `xAxisNameGap` | Sets spacing between the x-axis and its title. |
| `xAxisMin` | Sets x-axis minimum bound. |
| `xAxisMax` | Sets x-axis maximum bound. |
| `xAxisScale` | Allows the x-axis scale to avoid forcing zero where supported. |
| `xAxisStartValue` | Sets x-axis start value where supported. |
| `xAxisTickShow` | Shows or hides x-axis ticks. |
| `xAxisTickAlign` | Aligns x-axis ticks with labels when enabled. |
| `xAxisLabelShow` | Shows or hides x-axis labels. |
| `xAxisLabelSize` | Sets x-axis label font size. |
| `xAxisLabelFontFamily` | Sets x-axis label font family. See the font family values rule for accepted values. |
| `xAxisLabelInterval` | Controls x-axis label interval or skipping behavior. |
| `xAxisLabelInside` | Places x-axis labels inside the plot area when enabled. |
| `xAxisLabelRotate` | Rotates x-axis labels in degrees. |
| `xAxisLabelHideOverlap` | Hides overlapping x-axis labels when enabled. |
| `xAxisTriggerTooltip` | Allows x-axis labels to trigger tooltips. |
| `yAxisShow` | Shows or hides the y-axis. |
| `yAxisAlignTicks` | Aligns y-axis ticks across axes where supported. |
| `yAxisPosition` | Sets y-axis position. Valid values: "left", "right". |
| `yAxisType` | Sets the y-axis scale type. Valid values: "auto", "category", "value", "time", "log". |
| `yAxisName` | Sets y-axis title text. |
| `yAxisNameColor` | Sets y-axis title color. |
| `yAxisNameFontFamily` | Sets y-axis title font family. See the font family values rule for accepted values. |
| `yAxisNameSize` | Sets y-axis title font size. |
| `yAxisNameGap` | Sets spacing between the y-axis and its title. |
| `yAxisNameRotate` | Rotates the y-axis title in degrees. |
| `yAxisNameTruncateMaxWidth` | Limits y-axis title width before truncation. |
| `yAxisNameTruncateEllipsis` | Sets the ellipsis text for truncated y-axis title. |
| `yAxisNameMoveOverlap` | Moves y-axis title to avoid overlap when enabled. |
| `yAxisInverse` | Reverses y-axis direction when enabled. |
| `yAxisMin` | Sets y-axis minimum bound. |
| `yAxisMax` | Sets y-axis maximum bound. |
| `yAxisStartValue` | Sets y-axis start value where supported. |
| `yAxisTickShow` | Shows or hides y-axis ticks. |
| `yAxisTickAlign` | Aligns y-axis ticks with labels when enabled. |
| `yAxisTickInside` | Places y-axis ticks inside the plot area when enabled. |
| `yAxisLabelShow` | Shows or hides y-axis labels. |
| `yAxisLabelSize` | Sets y-axis label font size. |
| `yAxisLabelFontFamily` | Sets y-axis label font family. See the font family values rule for accepted values. |
| `yAxisLabelRotate` | Rotates y-axis labels in degrees. |
| `yAxisLabelInside` | Places y-axis labels inside the plot area when enabled. |
| `yAxisLabelPrefixEnabled` | Enables a prefix for y-axis labels. |
| `yAxisLabelPrefix` | Sets text before y-axis label values. |
| `yAxisLabelSuffixEnabled` | Enables a suffix for y-axis labels. |
| `yAxisLabelSuffix` | Sets text after y-axis label values. |
| `swapAxes` | Swaps the chart's x and y axes (line charts). For bar charts use bar.orientation="horizontal" instead. |
| `multiChart` | Splits compatible data into small multiple charts. |
| `showTotalLine` | Shows or hides the total/reference line. |
| `totalLineName` | Sets the display name for the total/reference line. |
| `tooltipContent` | Sets what content appears in chart tooltips. Valid values: "value", "group", "group_value". |
| `xField` | Sets the selected x-axis field from query output. |
| `yField` | Sets the selected y-axis value field from query output. |
| `multiSeriesFields` | Selects multiple query output fields to render as multiple series. Requires line.style="multi_series" in the same or an earlier change. |
| `xFormat` | Sets x-axis value formatting for both numbers and dates. Valid values: "string", "integer", "float", "short_date", "day_month_short", "day_month_long", "month_year_short", "month_year_long", "month_short", "month_long", "year", "long_date", "date_time". Pick a date format matching the data granularity (daily data: "short_date" or "day_month_short"; monthly: "month_year_short"; yearly: "year"). For "float", also set xFloatDecimals and xSeparator. |
| `yFormat` | Sets y-axis value formatting for both numbers and dates. Valid values: "string", "integer", "float", "short_date", "day_month_short", "day_month_long", "month_year_short", "month_year_long", "month_short", "month_long", "year", "long_date", "date_time". For "float", also set yFloatDecimals and ySeparator. |
| `xFloatDecimals` | Sets decimal places (0-8) for x-axis numeric values when xFormat is float. |
| `yFloatDecimals` | Sets decimal places (0-8) for y-axis numeric values when yFormat is float. |
| `xSeparator` | Sets the thousands separator for x-axis numeric values. Valid values: "none", ",", ".", "space". |
| `ySeparator` | Sets the thousands separator for y-axis numeric values. Valid values: "none", ",", ".", "space". |

### `bar` group

Used by widget kinds: `bar`, `combo`

| Key | Description |
| --- | --- |
| `categoryField` | Sets the bar category/grouping field from query output. |
| `valueField` | Sets the numeric bar value field from query output. |
| `seriesField` | Sets the optional bar series/split field from query output. |
| `seriesMode` | Controls how bar series are derived. Valid values: "group_by", "multi_series". "group_by" splits by seriesField; "multi_series" plots each field in multiSeriesFields. |
| `multiSeriesFields` | Selects multiple query output fields to render as bar series. Requires bar.seriesMode="multi_series". |
| `orientation` | Sets bar orientation. Valid values: "vertical", "horizontal". Use "horizontal" when the user asks for horizontal bars or to swap the axes of a bar chart. |
| `multiChart` | Splits compatible bar data into small multiple charts. |
| `stacked` | Stacks bar series when enabled. |
| `maxWidth` | Sets maximum bar width. |
| `borderRadius` | Sets bar corner radius. |

### `pie` group

Used by widget kinds: `pie`

| Key | Description |
| --- | --- |
| `style` | Sets pie rendering style. Valid values: "pie", "doughnut", "half_doughnut", "nightingale". |
| `labelType` | Controls which labels are shown on pie slices. Valid values: "value", "value_group", "group", "percent", "group_percent", "value_percent", "value_group_percent". |
| `sliceSort` | Controls pie slice sort order. Valid values: "data", "value_desc", "value_asc", "group_asc", "group_desc". |
| `outerLabelAlign` | Sets alignment for outer pie labels. Valid values: "natural", "edge". |
| `topNLimit` | Limits visible slices to the top N categories. |
| `topNOtherLabel` | Sets the label for grouped remaining slices. |
| `sliceSeparatorWidth` | Sets visual spacing between pie slices. |
| `percentLabelDecimals` | Sets decimal places for percentage labels. |
| `multiChart` | Splits compatible pie data into small multiple charts. |

### `funnel` group

Used by widget kinds: `funnel`

| Key | Description |
| --- | --- |
| `stageField` | Sets the funnel stage/category field from query output. |
| `valueField` | Sets the funnel numeric value field from query output. |
| `labelMode` | Controls which labels appear on funnel stages. Valid values: "value", "value_conversion", "value_dropoff". |
| `labelColor` | Sets funnel label text color. |

### `radar` group

Used by widget kinds: `radar`

| Key | Description |
| --- | --- |
| `shape` | Sets radar chart shape. Valid values: "polygon", "circle". |
| `areaOpacity` | Sets filled radar area opacity. |
| `splitNumber` | Sets the number of radar grid rings/splits. |
| `axisLabelShow` | Shows or hides radar axis labels. |
| `multiChart` | Splits compatible radar data into small multiple charts. |

### `sankey` group

Used by widget kinds: `sankey`

| Key | Description |
| --- | --- |
| `sourceField` | Sets the sankey source node field from query output. |
| `targetField` | Sets the sankey target node field from query output. |
| `valueField` | Sets the sankey link value/weight field from query output. |
| `nodeWidth` | Sets sankey node width. |
| `nodeGap` | Sets spacing between sankey nodes. |
| `nodeAlign` | Sets sankey node alignment. Valid values: "justify", "left", "right". |
| `linkColorMode` | Controls how sankey links are colored. Valid values: "source", "target", "gradient". |
| `linkOpacity` | Sets sankey link opacity. |
| `curveness` | Sets sankey link curve amount. |
| `draggable` | Allows users to drag sankey nodes when enabled. |
| `showNodeLabels` | Shows or hides sankey node labels. |
| `labelColor` | Sets sankey label text color. |

### `kpi` group

Used by widget kinds: `kpi`

| Key | Description |
| --- | --- |
| `alignment` | Sets KPI content alignment. |
| `headerColor` | Sets KPI header/label text color. |
| `headerSize` | Sets KPI header/label font size. |
| `headerWeight` | Sets KPI header/label font weight. |
| `headerStyle` | Sets KPI header/label font style. |
| `valueColor` | Sets KPI value text color. |
| `valueSize` | Sets KPI value font size. |
| `valueWeight` | Sets KPI value font weight. |
| `valueStyle` | Sets KPI value font style. |

### `dataGrid` group

Used by widget kinds: `dataGrid`

| Key | Description |
| --- | --- |
| `pageSize` | Sets how many rows the data grid displays per page. |
| `density` | Sets compact or comfortable row density. |
| `zebraRows` | Alternates row backgrounds when enabled. |
| `stickyHeader` | Keeps the grid header visible while scrolling when enabled. |
| `allowColumnSort` | Allows users to sort grid columns when enabled. |
| `showRowRange` | Shows the visible row range/count when enabled. |
| `truncateCells` | Truncates overflowing cell content when enabled. |
| `defaultSort` | Sets initial grid sort as { field, direction }. |
| `columns` | Configures grid columns as [{ key, label, hidden, width }] for labels, visibility, and width. |
| `editable` | Configures grid editing as { enabled, keyFields } for editable rows. |
| `conditionalFormatting` | Configures data grid visual conditional formatting as { rules }, where rules may be condition rules or numeric colorScale rules targeting cells, columns, or rows with controlled style fields only. |

### `pivotTable` group

Used by widget kinds: `pivotTable`

| Key | Description |
| --- | --- |
| `rows` | Sets pivot row dimension fields from query output. |
| `columns` | Sets pivot column dimension fields from query output. |
| `measures` | Sets pivot measures as [{ id, field, aggregate, label }] using SUM, COUNT, DISTINCT_COUNT, AVG, MIN, or MAX. |
| `showRowSubtotals` | Shows or hides row subtotals. |
| `showColumnSubtotals` | Shows or hides column subtotals. |
| `showRowGrandTotal` | Shows or hides the row grand total. |
| `showColumnGrandTotal` | Shows or hides the column grand total. |
| `nullValueDisplay` | Displays null values as blank or zero. |
| `clickFiltering` | Controls pivot cross-filtering as { rowHeaders, columnHeaders, valueCells }. |
| `valueSort` | Sorts pivot values as { columnKey, direction }. |
| `valueFilters` | Filters pivot values as [{ id, columnKey, operator, value }]. |
| `defaultExpansion` | Sets pivot groups initially expanded or collapsed. |
| `rowHeaderWidthPx` | Sets row header column width in pixels. |
| `defaultValueColumnWidthPx` | Sets default pivot value column width in pixels. |
| `valueColumnWidths` | Overrides individual pivot value column widths as [{ key, width }]. |

### `maps` group

Used by widget kinds: `maps`

| Key | Description |
| --- | --- |
| `map` | Configures base map options: centerMode, centerLat, centerLng, zoom, and mapTypeId. |
| `layers` | Configures map layers, including markers, heatmap, polygon, route, fields, labels, tooltips, styling, and cross-filtering. |

### `summary` group

Used by widget kinds: `summary`

| Key | Description |
| --- | --- |
| `mode` | Sets summary source mode. |
| `contextPrompt` | Sets the instruction/context text used to generate the summary. |
| `referenceWidgetId` | Sets the referenced widget id for widget-based summaries. |
| `referenceBoardId` | Sets the referenced board id for board or panel summaries. |
| `referencePanelId` | Sets the referenced panel id for panel summaries. |

### `customHtml` group

Used by widget kinds: `customHtml`

| Key | Description |
| --- | --- |
| `template` | Sets the readable multi-line Handlebars HTML template rendered in the custom HTML widget. The sandboxed renderer allows inline CSS and JavaScript <script> tags, and exposes theme CSS variables for readable light/dark output. |

### `freeText` group

Used by widget kinds: `freeText`

| Key | Description |
| --- | --- |
| `content` | Sets the sanitized free-text HTML content. |
| `fontSize` | Sets free-text font size. |
| `fontFamily` | Sets free-text font family from supported options. |
| `alignment` | Sets free-text alignment. |
| `color` | Sets free-text text color. |

## Enum value sets

| Enum | Values |
| --- | --- |
| `lineStyle` | `"basic"`, `"smooth"`, `"area"`, `"stacked_area"`, `"stacked_line"`, `"multi_series"` |
| `titleMode` | `"none"`, `"custom"`, `"group"` |
| `titleFontStyle` | `"normal"`, `"italic"` |
| `titleFontWeight` | `"normal"`, `"500"`, `"600"`, `"700"`, `"800"` |
| `titlePlacement` | `"top_left"`, `"top_center"`, `"top_right"`, `"bottom_left"`, `"bottom_center"`, `"bottom_right"` |
| `legendPlacement` | `"top_left"`, `"top_center"`, `"top_right"`, `"middle_left"`, `"middle_right"`, `"bottom_left"`, `"bottom_center"`, `"bottom_right"` |
| `legendIcon` | `"circle"`, `"rect"`, `"roundRect"`, `"triangle"`, `"diamond"`, `"pin"`, `"arrow"`, `"none"` |
| `colorPaletteStrategy` | `"cycle"`, `"tint_shade"`, `"hue_shift"`, `"hash_series"` |
| `pointLabelPosition` | `"top"`, `"bottom"`, `"left"`, `"right"`, `"inside"`, `"insideTop"`, `"insideBottom"`, `"insideLeft"`, `"insideRight"` |
| `pointLabelFormat` | `"none"`, `"prefix"`, `"suffix"` |
| `tooltipContent` | `"value"`, `"group"`, `"group_value"` |
| `xAxisPosition` | `"bottom"`, `"top"` |
| `yAxisPosition` | `"left"`, `"right"` |
| `axisType` | `"auto"`, `"category"`, `"value"`, `"time"`, `"log"` |
| `axisFormat` | `"string"`, `"integer"`, `"float"`, `"short_date"`, `"day_month_short"`, `"day_month_long"`, `"month_year_short"`, `"month_year_long"`, `"month_short"`, `"month_long"`, `"year"`, `"long_date"`, `"date_time"` |
| `numericSeparator` | `"none"`, `","`, `"."`, `"space"` |
| `cartesianMode` | `"bar"`, `"line"`, `"combo"` |
| `comboSeriesType` | `"bar"`, `"line"` |
| `comboSeriesAxis` | `"left"`, `"right"` |
| `barSeriesMode` | `"group_by"`, `"multi_series"` |
| `barOrientation` | `"vertical"`, `"horizontal"` |
| `pieStyle` | `"pie"`, `"doughnut"`, `"half_doughnut"`, `"nightingale"` |
| `pieLabelType` | `"value"`, `"value_group"`, `"group"`, `"percent"`, `"group_percent"`, `"value_percent"`, `"value_group_percent"` |
| `pieSliceSort` | `"data"`, `"value_desc"`, `"value_asc"`, `"group_asc"`, `"group_desc"` |
| `pieOuterLabelAlign` | `"natural"`, `"edge"` |
| `funnelLabelMode` | `"value"`, `"value_conversion"`, `"value_dropoff"` |
| `radarShape` | `"polygon"`, `"circle"` |
| `sankeyNodeAlign` | `"justify"`, `"left"`, `"right"` |
| `sankeyLinkColorMode` | `"source"`, `"target"`, `"gradient"` |
