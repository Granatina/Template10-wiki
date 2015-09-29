# Converters

Template10 ships with a collection of useful converters that you can use in your application.
- [DateTimeFormatConverter](#datetimeformatconverter)
- [ValueWhenConverter](#valuewhenconverter)

## <a name="datetimeformatconverter" /> DateTimeFormatConverter

### Contents Overview

- [Implementation](#datetimeformatconverter-implementation)

This converter can take in a DateTime value and format it using a  DateTime format string.  You can find a list of the Standard Date and Time format strings at MSDN: [http://msdn.microsoft.com/en-us/library/az4se3k1.aspx](http://msdn.microsoft.com/en-us/library/az4se3k1.aspx). You can also build you own custom Date and Time format string using the infromation from this MSDN article: [http://msdn.microsoft.com/en-us/library/8kb3ddd4.aspx](http://msdn.microsoft.com/en-us/library/8kb3ddd4.aspx).

### <a name="datetimeformatconverter-implementation" /> Implementation

You can add this control as a Resource to another XAML element:

    <Page.Resources>
        <converters:DateTimeFormatConverter x:Key="DTFormatConverter" />
    </Page.Resources>

With the resource in place, you can use the resource as the **Converter** when binding a value on your page. The **ConverterParameter** binding property specifies the format string:

    <TextBlock Text="{Binding DateTimeValue, Converter={StaticResource DTFormatConverter}, ConverterParameter=D}" />

## <a name="valuewhenconverter" /> ValueWhenConverter

### Contents Overview

- [Properties](#valuewhenconverter-properties)
- [Implementation](#valuewhenconverter-implementation)

This converter can display data from one of two binary choices.  If the data being bound is equivalent to the **When** property of the converter, then the result of the binding will be the **Value** property of the converter.  If they are not equivalent, the result of the binding will be the **Otherwise** converter.

### <a name="valuewhenconverter-properties" />

- **When** (object)
> This is the object evaluated for equivalence with the bound value.  The bound value is technically an input parameter to this converter.

- **Value** (object)
> This object is the result of the binding conversion if the originally bound value is equivalent to the value of the **When** property.

- **Otherwise** (object)
> This object is the result of the binding conversion if the originally bound value is **NOT** equivalent to the value of the **When** property.

### <a name="valuewhenconverter-implementation" /> Implementation

You can add this binding as a Resource to another XAML element.

    <converters:ValueWhenConverter 
        Value="The bound value is the string 'Test'" 
        Otherwise="The bound value is something other than the string 'Test'"
        When="Test"
        x:Key="VWConverter" />

With the resource in place, you can use the resource as the **Converter** when binding a value on your page.

    <TextBlock Text="{Binding ExternalValue, Converter={StaticResource VWConverter}}" />