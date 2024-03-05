#### 5th 
- ##### Interview question #1:

  There is a product whose price may change.
  The price is set using an array, the elements of which contain the price and the date from which the price becomes effective.
  Write a function that returns the price for a specified date (function accepts array of prices and date).
  ```
  public record PriceDescriptor(DateTime ValidFrom, decimal Price);
  public decimal? FindPrice(PriceDescriptor[] prices, DateTime validAt) => prices
      .Where(d => d.ValidFrom <= validAt)
      .MaxBy(d => d.ValidFrom)?.Price;
  ```
  Improvements:
   - Do it in a single loop (without LINQ)
   - Make caller pass sorted array and use binary search
   - B+ trees?  
-  ##### Interview question #2:
