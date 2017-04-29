Gopandas is an experimental golang package that aims to use "R/Python DataFrames".
This package is full inspirated by the pandas package in python and try to reproduce some of his functionalities as:

- Have a structure of data organized in columns (called a DataFrame)
- Create a DataFrame from a CSV or JSON file (IN PROGRESS)
- Convert a DataFrame into CSV or JSON file (TO DO)
- Selection by columns
- Filtering by logical operator as AND, OR, GREAT, LESS, EQUAL (TO DO)
- Group by (TO DO)
- Join (TO DO)
- Apply functions (TO DO)
- Statistical functions (TO DO)

This package has been develop with the intention to be usable by machine learning packages in go.

**Output representation of a dataframe use the [github.com/olekukonko/tablewriter](https://github.com/olekukonko/tablewriter) package**

**/!\ THIS PACKAGE IS STILL UNDER DEVELOPMENT. A LOT OF FUNCTIONALITIES IS MISSING AND/OR COULD BE CHANGED /!\ **

# Installation

```
go get github.com/olekukonko/tablewriter
go get github.com/fmarmol/gopandas
```

# C interface
All gopandas types implements the C interface

```
type C interface {
    Add(C) C
    Mul(C) C
    Div(C) C
    Mod(C) C
    Great(C) bool
    Less(C) bool
    Equal(C) bool
    NotEqual(C) bool
}
```

# Series

Series are basically maps of values with indices as keys.

## Initialize series
Series can be construct with maps or slices of values. Maps are used as input if there is a need to specify the indices of the values. Indices avec empty interfaces so feel free to use what you want. If slices are used the indices will be the positions in the slices of the values.
Gopandas will convert automatically all types that are compatibles with the section types. 

```
import
    (
    "fmt"
    "github.com/fmarmol/gopandas/series"
)

func main() {
    s1 := series.NewSeries([]int{1,2,3})
    s2 := series.NewSeries(map[series.Index]interface{}{"one":1, 2:"two"})
    fmt.Print(s1)
    fmt.Print(s2)
}

//output:
Series:{0:1, 1:2, 2:3}
Series:{one:1, 2:two}
```

## Usage of series

- You can check types insides a series with Type method, this will give you a simple summary

```
fmt.Println(s1.Type())
fmt.Println(s2.Type())
//output:
map[numeric:3]
map[string:1 numeric:1]
```

- You can have the number of occurences by values inside a series with ValuesCount method
```
fmt.Println(series.NewSeries([]float64{1.1,1.2,1.3,1.4,1.3,1.2}).ValuesCount())
//output:
map[1.1:1 1.3:2 1.4:1 1.2:2]
```

- You can apply function on series with method Apply. You need to pass a func(types.C) types.C, and it returns
a new series with values modified by the function:

```
fmt.Print(
    series.NewSeries([]float64{1.1, 1.2}).Apply(
        func(c types.C) types.C {
            return c.Add(types.Numeric(1))
        }),
)
//output:
Series:{0:2.1, 1:2.2}
```

# Types

Gopandas manages few types representing the most common types:

- Time for time (constructed over time.Time)
- Numeric for numbers (constructed over float64)
- String for string (constructed over string)
- Nan for Not A Number (constructed over string)

**Note**: All Types of Gopandas implement the gopandas/types.C interface
