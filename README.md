# Clean Code with Delphi (Short Guide)

**Clean Code**, describes the practice of writing code that is readily readable, understandable, maintainable, and modifiable. It goes beyond simply adhering to style conventions and includes applying design principles that promote long-term code comprehension and maintainability. This short guide will review some key concepts.

## Efficient Naming
Meaningful names are at the heart of clean code. In Object Pascal (Delphi):
- **InfixCaps**: Use InfixCaps naming convention.
- **Variables:** Use intention-revealing names (`UserAge` instead of `a, Tmp, vName`).
- **Methods:** Names should clearly express purpose (`CalculateTotalPrice`).
- **Properties:** Name properties descriptively to reflect their purpose (`CustomerName` rather than `CN`)
- **Type Prefix**: Prefix all classes and custom types with a `T letter` (`TCustomer`), for exception classes `E letter` (`ECustomerType`) and interface types `I letter` (`ICustomerResponse`).
- **Classes:** Avoid ambiguity by using nouns (`InvoiceProcessor`).
- **Constants:** Avoid underscores & symbols (`DefaultCustomerAge: Integer = 18`).
- **Reserved Words**: Language keywords, reserved words, and compiler directives must all be lowercase.
- **Use English**: Always name variables, methods, and classes in English.

## Functions/Methods/Properties Declaration
Small, focused methods enhance clarity. Follow these principles:
- **Single Responsibility:** Each method should handle only one task.
- **Avoid Side Effects:** Predictable methods yield cleaner interactions.
- **Avoid direct field access:** Use properties to enforce validation, computed values, or controlled modification.
- **Define read-only properties:** When values shouldn’t be modified after initialization.

## Comments
Comments are powerful but should be used sparingly:
- **Avoid Redundancy:** Don’t comment on obvious code.
- **Explain Intent:** Focus on why something is done, not what is done.
- **TODO Notes:** Document areas requiring improvement.

### Example:
```pascal
// Adjusts prices based on inflation rate
procedure UpdatePrices;
begin
  // Complex business logic here
end;
```

## Formatting
Consistency and readability are crucial:
- **Indentation:** Standardize across the team.
- **Line Breaks:** Use them generously to enhance readability.
- **Block Alignment:** Ensure clarity in nested structures.

### Example:
```pascal
if Condition then
begin
  ExecuteAction;
end
else
begin
  HandleError;
end;
```

## Class, Objects and Data Structures
Cohesive classes and proper encapsulation are essential:
- **Encapsulation:** Use public, private or protected access modifiers to hide implementation details.
- **Avoid Large Classes:** Break down large classes into smaller, specialized ones to maintain focus and simplicity.
- **Constructor Use:** Initialize fields properly.
- **Use Destructor to Free Memory:** Always pair the constructor with a destructor to clean up resources and manage memory efficiently.


### Example:
```pascal
type
  TCustomer = class
  private
    FName: string;
    procedure SetName(Value: string);
  public
    constructor Create(Name: string);
    destructor Destroy; override;
    property Name: string read FName write SetName;
  end;

constructor TCustomer.Create(Name: string);
begin
  FName := Name;
end;

destructor TCustomer.Destroy;
begin
  inherited;
end;

procedure TCustomer.SetName(Value: string);
begin
  if Value <> '' then
  begin
   FName := Value
  end;  
end;

```

## Error Handling
Clean error handling safeguards code integrity:
- **Check Resource Availability**: Always verify whether a resource exists or is available before trying to use it. This can prevent exceptions and ensure that the program continues running smoothly.
- **Exceptions:** Use `try...except or finally` blocks effectively.
- **Meaningful Messages:** Provide context in error messages.
- **Avoid Silent Errors:** Always report issues explicitly.

### Example:
```pascal
var
  Customer: TCustomer;

begin
  try
    if Assigned(Customer) then
      ShowMessage('Customer Name: ' + Customer.Name)
    else
      raise Exception.Create('Error: Customer object is not initialized.');
  except
    on E: Exception do
    begin
      ShowMessage('Exception Caught: ' + E.Message);
    end;
end;
```

## FireMonkey / Multiplatform Application
FireMonkey (FMX) is Delphi's cross-platform framework, enabling the development of applications for Windows, macOS, iOS, Android, and more. Here are key recommendations:
- **Platform-Specific Code:** Leverage Delphi’s compiler directives to conditionally compile code for different platforms. This ensures clarity and avoids unnecessary clutter in shared units.
- **Separate Units for Platform:** Organize platform-specific implementations into separate units. Use naming conventions such as `UnitName.Android.pas, UnitName.iOS.pas, etc.`, for clarity and consistency.
- **Optimize Performance for Mobile:** Mobile platforms are more resource-constrained compared to desktops. Use Platform Services and Interfaces.

### Example:
```pascal
procedure PlatformSpecificFeature;
begin
  {$IFDEF MSWINDOWS}
  ShowMessage('Windows-specific code here');
  {$ENDIF}

  {$IFDEF ANDROID}
  ShowMessage('Android-specific code here');
  {$ENDIF}
end;
```

## Testing
Unit testing is a cornerstone of clean code:
- **Test Coverage:** Aim to cover critical functionality.
- **Readable Tests:** Tests should be as clean as the code they verify.
- **Automate:** Integrate tests into your build process.

### Example:
```pascal
procedure TestCalculateTotalPrice;
begin
  Assert.AreEqual(50, FCalculator.CalculateTotalPrice(5, 10), 'The price calculation is incorrect');
end;
```

## Share
If you liked and found this repository useful for your projects, star it. Thank you for your support! ⭐
