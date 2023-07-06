## 6th

- Interview questions
  - Imagine you are a compiler:
    ```csharp
    using System;
    
    namespace Test
    {
        public interface ISomeInterface
        {
            void DoNothing();
        }
    	
        public class OtherClass : ISomeInterface
        {
            public void DoNothing()	{ }
        }
    	
        public abstract class BaseClass<T>
        {
            protected T par;
    	
            public BaseClass(T par)
            {
                this.par = par;
            }
    		
            public virtual void myfunc()
            {
                this.par.DoNothing();
            }
        }
    
        public class MyClass : BaseClass
        {
            private int id { public get; set; }
    		
            private int _index;
            public int index
            {
                get;
                set
                {
                    if (value > 0)
                        this._index = value;
                }
            }
            protected string name;
            public event EventHandler initevent;
    
            public MyClass()
                : base(new OtherClass())
            {
                this.initevent(this, null);
            }
    
            protected virtual void SomeFunc() 
            { }
        }
    
        public class MyNewClass : MyClass
        {
            private const int someConst;
            private readonly int someReadonly;
    
            public MyNewClass()
                : base()
            {
                this.id = 10;
                this.index = 5;
                this.name = "New";
    
                this.someConst = 2;
                this.someReadonly = 2;
    
                this.initevent(this, null);
            }
    
            private override void SomeFunc()
            { }
    
            public MyNewClass ConvertToMyClass(object val)
            {
                return val is MyNewClass;
            }
        }
    }
    ```
  - "Implement"/refactor (simulate calls to other services)
    ```csharp
    namespace test;
    using System.Text.RegularExpressions;
    public class KingJson
    {
        public string GetDescription(int itemId)
        {
            Thread.Sleep(1000);
            return "☆☆☆Lorem ♡♡♡ ipsum  dolor sit amet...";
        }
    
        public string Prettify(string description)
        {
            Thread.Sleep(1000);
            var rx = new Regex("\\W+",RegexOptions.Compiled | RegexOptions.IgnoreCase);
            return rx.Replace(description, string.Empty);
        }
        
        public float GetPrice(int itemId)
        {
            Thread.Sleep(2000);
            return 100;
        }
    
        public float PriceToRub(float price)
        {
            Thread.Sleep(2000);
            return price * 70;
        }
        
        public Snippet NewSnippet(float price, string description)
        {
            // default behavior
            return new Snippet();
        }
    
        public Snippet BuildSnippet(int itemId)
        {
            // default behavior
            return NewSnippet(default(float), string.Empty);
        }
    }
    
    public class Snippet
    {    
    }
    ```
  - "What will happen next"
    ```csharp
    using System.Threading.Tasks;
    
    Task.Run(() =>
    {
        var rrr = GetNumber(1);
        rrr.ConfigureAwait(true);
        Console.WriteLine(rrr.Result);
    });
    Task.Run(() =>
    {
        var rrr = GetNumber(2);
        rrr.ConfigureAwait(true);
        Console.WriteLine(rrr.Result);
    });
    Console.WriteLine(await GetNumber(3));
    Console.WriteLine("Hello, World!");
    Console.WriteLine(System.Environment.UserName);
    
    static async ValueTask<int> GetNumber(int x)
    {
        return x;
    }
    ```
- Meme of the stream
  ![image](https://github.com/gurustron/stream-notes/assets/6535969/ba1129f0-7c15-437d-a5b5-486358b2972d)
