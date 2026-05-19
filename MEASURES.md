# 🧮 DAX Measures Documentation
> BMW Sales Analysis — Power BI Report

---

## 📦 Categories Overview

| Category | Measures |
|---|---|
| 💰 Revenue | Revenue, RevenuePY, Revenue Variance, Revenue Growth, Revenue Variance % Arrow |
| 📦 Quantity | Qty Sold, Qty Sold PY, Qty Sold Variance, Qty Sold Growth |
| 💲 Pricing | Avg Price, Avg Price Formatted |
| 📈 Sparklines | Revenue Sparkline, Opaque Area Sparkline |
| 🗓️ Utility | Selected Period |

---

## 💰 Revenue Measures

---

### `Revenue`
**Purpose:** The core revenue KPI — sums all revenue from the fact table. Used as the base for all revenue-related calculations.

```dax
Revenue = 
    SUM('Fact Table'[Total])
```

---

### `RevenuePY`
**Purpose:** Returns total revenue for the same period in the **prior year** using time intelligence. Used as the baseline for YoY comparisons.

```dax
RevenuePY = 
    CALCULATE(
        [Revenue],
        DATEADD(
            'Calender'[Date],
            -1, YEAR)
    )
```

---

### `Revenue Variance`
**Purpose:** The absolute difference between current revenue and prior year revenue. Tells you how much revenue grew or declined in raw numbers.

```dax
Revenue Variance = 
    [Revenue] - [RevenuePY]
```

---

### `Revenue Growth`
**Purpose:** Calculates the **percentage growth** in revenue versus the prior year. Used in KPI cards and trend visuals.

```dax
Revenue Growth = 
    DIVIDE(
        [Revenue Variance],
        [RevenuePY]
    )
```

---

### `Revenue Variance % Arrow`
**Purpose:** A **formatted display measure** that combines the growth percentage with an up/down arrow emoji (▲/▼) for intuitive KPI cards. Returns BLANK if there's no revenue data.

```dax
Revenue Variance % Arrow = 
    VAR _upArrow = UNICHAR(129129)
    VAR _downArrow = UNICHAR(129131)

    RETURN
        IF(
            ISBLANK([Revenue]),
            BLANK(),
            IF(
                [Revenue Variance] > 0,
                "+" & ROUND([Revenue Growth] * 100, 1) & "%" & _upArrow,
                ROUND([Revenue Growth] * 100, 1) & "%" & _downArrow
            )
        )
```

---

## 📦 Quantity Measures

---

### `Qty Sold`
**Purpose:** The core volume KPI — total units sold. Used as the base for all quantity-related calculations.

```dax
Qty Sold = 
    SUM('Fact Table'[Quantity Sold])
```

---

### `Qty Sold PY`
**Purpose:** Returns total quantity sold for the same period in the **prior year**. Used as the baseline for volume YoY comparisons.

```dax
Qty Sold PY = 
    CALCULATE(
        [Qty Sold],
        DATEADD(
            'Calender'[Date],
            -1,
            YEAR
        )
    )
```

---

### `Qty Sold Variance`
**Purpose:** Absolute difference in units sold between the current period and the prior year.

```dax
Qty Sold Variance = 
    [Qty Sold] - [Qty Sold PY]
```

---

### `Qty Sold Growth`
**Purpose:** Percentage growth in units sold versus the prior year. Mirrors Revenue Growth but for volume.

```dax
Qty Sold Growth = 
    DIVIDE(
        [Qty Sold Variance],
        [Qty Sold PY]
    )
```

---

## 💲 Pricing Measures

---

### `Avg Price`
**Purpose:** Calculates the **average selling price per unit** by dividing total revenue by total quantity sold. Useful for tracking pricing trends by model, region, or channel.

```dax
Avg Price = 
    DIVIDE(
        [Revenue],
        [Qty Sold]
    )
```

---

### `Avg Price Formatted`
**Purpose:** A **display-ready version** of Avg Price, formatted as `$XXX.XK` for clean presentation in KPI cards and tooltips.

```dax
Avg Price Formatted = 
    FORMAT([Avg price], "$#,.0,K")
```

---

## 📈 Sparkline Measures

---

### `Revenue Sparkline`
**Purpose:** Generates an **inline SVG sparkline chart** of monthly revenue trends. Designed to be embedded inside a table or matrix visual as an Image URL column, giving each row a mini trend line without needing a separate chart.

```dax
Revenue Sparkline = 

VAR XMinDate = MIN(Calender[Monthnum])
VAR XMaxDate = MAX(Calender[Monthnum])

VAR YMinValue =
    MINX(
        VALUES(Calender[Monthnum]),
        CALCULATE([Revenue])
    )

VAR YMaxValue =
    MAXX(
        VALUES(Calender[Monthnum]),
        CALCULATE([Revenue])
    )

VAR SparklineTable =
    ADDCOLUMNS(
        SUMMARIZE('Calender', Calender[Monthnum]),
        "X",
            INT(
                150 * DIVIDE(
                    Calender[Monthnum] - XMinDate,
                    XMaxDate - XMinDate
                )
            ),
        "Y",
            INT(
                50 * DIVIDE(
                    CALCULATE([Revenue]) - YMinValue,
                    YMaxValue - YMinValue
                )
            )
    )

VAR Lines =
    CONCATENATEX(
        SparklineTable,
        [X] & "," & 50 - [Y],
        " ",
        Calender[Monthnum]
    )

VAR SVGImageURL =
    "data:image/svg+xml;utf8," &
    "<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 150 50'>" &
    "<polyline fill='navy' fill-opacity='0.3' stroke='navy' stroke-width='3' points='0 50 " &
    Lines &
    " 150 150 Z'/></svg>"

RETURN
    SVGImageURL
```

> ⚠️ **Setup Required:** Set the Data Category of this measure to **Image URL** in Power BI Model view.

---

### `Opaque Area Sparkline (no one value) bright`
**Purpose:** An **enhanced SVG sparkline** for Quantity Sold with a gradient fill, a highlighted last data point (circle marker), and a bright blue color scheme (`#118DFF`). More visually polished than the basic sparkline. Derived from Eldersveld's technique, modified by Kolosko.

```dax
Opaque Area Sparkline (no one value) bright = 
VAR LineColour = "%23118DFF"
VAR PointColour = "white"
VAR Defs = "<defs>
    <linearGradient id='grad' x1='0' y1='25' x2='0' y2='50' gradientUnits='userSpaceOnUse'>
        <stop stop-color='#118DFF' offset='0' />
        <stop stop-color='#118DFF' offset='0.3' />
        <stop stop-color='white' offset='1' />
    </linearGradient>
</defs>"
VAR XMinDate = MIN(Calender[Monthnum])
VAR XMaxDate = MAX(Calender[Monthnum])
VAR YMinValue = MINX(Values(Calender[Monthnum]), CALCULATE([Qty Sold]))
VAR YMaxValue = MAXX(Values(Calender[Monthnum]), CALCULATE([Qty Sold]))
VAR SparklineTable = ADDCOLUMNS(
    SUMMARIZE('Calender', Calender[Monthnum]),
        "X", INT(150 * DIVIDE(Calender[Monthnum] - XMinDate, XMaxDate - XMinDate)),
        "Y", INT(50 * DIVIDE([Qty Sold] - YMinValue, YMaxValue - YMinValue)))
VAR Lines = CONCATENATEX(SparklineTable, [X] & "," & 50-[Y], " ", Calender[Monthnum])
VAR LastSparkYValue = MAXX(FILTER(SparklineTable, Calender[Monthnum] = XMaxDate), [Y])
VAR LastSparkXValue = MAXX(FILTER(SparklineTable, Calender[Monthnum] = XMaxDate), [X])
VAR SVGImageURL = 
    "data:image/svg+xml;utf8," & 
    "<svg xmlns='http://www.w3.org/2000/svg' x='0px' y='0px' viewBox='-7 -7 164 64'>" & Defs & 
    "<polyline fill='url(#grad)' fill-opacity='0.3' stroke='transparent' stroke-width='0' points=' 0 50 " & Lines & " 150 150 Z '/>" &
    "<polyline fill='transparent' stroke='" & LineColour & "' stroke-linecap='round' stroke-linejoin='round' stroke-width='3' points=' " & Lines & " '/>" &
    "<circle cx='"& LastSparkXValue & "' cy='" & 50 - LastSparkYValue & "' r='4' stroke='" & LineColour & "' stroke-width='3' fill='" & PointColour & "' />" &
    "</svg>"
RETURN SVGImageURL
```

> ⚠️ **Setup Required:** Set the Data Category of this measure to **Image URL** in Power BI Model view. Uses `%23` instead of `#` for Firefox compatibility.

---

## 🗓️ Utility Measures

---

### `Selected Period`
**Purpose:** Displays the **currently selected date range** as a formatted string (e.g., `Jan 1, 2023 - Dec 31, 2023`). Used in report headers or subtitles to tell the viewer what time window they're looking at.

```dax
Seleceted Period = 
    FORMAT(MIN('Calender'[Date]), "MMM D, YYYY") & " - " & FORMAT(MAX('Calender'[Date]), "MMM D, YYYY")
```

---

## 🔗 Dependencies

| Measure | Depends On |
|---|---|
| Revenue Variance | Revenue, RevenuePY |
| Revenue Growth | Revenue Variance, RevenuePY |
| Revenue Variance % Arrow | Revenue, Revenue Variance, Revenue Growth |
| Qty Sold Variance | Qty Sold, Qty Sold PY |
| Qty Sold Growth | Qty Sold Variance, Qty Sold PY |
| Avg Price | Revenue, Qty Sold |
| Avg Price Formatted | Avg Price |
| Revenue Sparkline | Revenue, Calender[Monthnum] |
| Opaque Area Sparkline | Qty Sold, Calender[Monthnum] |
