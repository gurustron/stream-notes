#### 12th
- `AsNoTrackingWithIdentityResolution` https://stackoverflow.com/a/78143863/2501279

#### 11th
- CDC Meeting Report Tool - https://github.com/gurustron/cdc-meeting-report-tool
- ![image](https://github.com/gurustron/stream-notes/assets/6535969/52b8a69a-9778-4fc2-887d-cbb1d1765b7f)


#### 5th 
- [HTTPCLIENT CONNECTION POOLING IN .NET CORE](https://www.stevejgordon.co.uk/httpclient-connection-pooling-in-dotnet-core)
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
    ```csharp
        public class MyPlane
    {
        private int FlightId;
        private PersonInPlane PlanePilot;
        private int _currentNumber = 0;
        private string coordinate;

        public MyPlane(PersonInPlane p, int id)
        {
            PlanePilot = p;
            FlightId = id;
            Passengers = new ConcurrentDictionary<int, PersonInPlane>();
        }
        public void Register(PersonInPlane p)
        {
            Passengers[_currentNumber++] = p; 
        }
        private static object _sync = new object();
        
        public async void CheckPlaneCoordinate()
        {
            Monitor.Enter(_sync); 
            try
            {
                string temp = await Navigator.GetCoordinate();
                if (!coordinate.Equals(temp))
                {
                    coordinate = temp;
                }
            }
            catch (Exception e)
            {
                Monitor.Exit(_sync);
            }
        }
        
        public void ChangePlanePilot(string fn, string ln, string doc)
        {
            PlanePilot.FirstName = fn;
            PlanePilot.LastName = ln;
        }
        
        public async Task StartPlaneFlight()
        {
            await FlightDb.Start(FlightId, JsonConvert.SerializeObject(Passengers));
            await FlightKafkaQueue.SendStart(FlightId, JsonConvert.SerializeObject(Passengers));
        }
        public ConcurrentDictionary<int, PersonInPlane> Passengers { get; init; }
    }

    public class PersonInPlane
    {
        public string DocumentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
    ```
-  ##### Interview question #3:

