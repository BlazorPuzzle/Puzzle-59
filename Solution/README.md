# Blazor Puzzle #59

## Coding for Dollars

YouTube Video: https://youtu.be/

Blazor Puzzle Home Page: https://blazorpuzzle.com

### The Challenge:

This is a .NET 9 Blazor Web App with global server interactivity.

Our customer has requested we allow currency (decimal) values to be formatted with a $, and made editable.

We can not use a third-party tool. We must use a standard HTML input tag.

How can we allow editing and still format the value as currency rounded to two decimal places and including a dollar sign?

Non-numeric values should be ignored, as should numbers two places to the right of the decimal point.

### Solution:

There are a few ways to solve this, but we chose to do it by binding to a string property that does the formatting.

Here's the code:

```c#
decimal MyCurrencyValue { get; set; } = 0;

string EditableCurrencyValue
{
	get => MyCurrencyValue.ToString("C");
	set
	{
    	var newValue = CleanAndFormatDecimal(value, 2);
    	if (newValue != 0)
        	MyCurrencyValue = newValue;
	}
}

decimal CleanAndFormatDecimal(string input, int decimalPlaces)
{
	// Try to parse the input string as a decimal
	if (decimal.TryParse(input, out decimal result))
	{
    	// Format the decimal with the specified number of decimal places
	    var formattedString = Math.Round(result, decimalPlaces).ToString($"F{decimalPlaces}");
	    if (formattedString != "")
    	    return decimal.Parse(formattedString);
	}

	// If parsing fails return 0
	return 0;
}
```

Here's the markup:

```html
<input onfocus="this.select()" style="font-weight:bold;text-align:right;"
       @bind="@EditableCurrencyValue" />
```

The magic sauce is the `CleanAndFormatDecimal` method, which can be coded in a number of ways. The idea is to remove all non-numeric characters and try converting to a decimal. If it converts, we convert it to a decimal and return it.

Boom!
