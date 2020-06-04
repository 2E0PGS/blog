
in old test you dont have the Assert.Throws
So use below try catch return

old MS test
```

        [TestMethod]
        public void GetStuff_ThrowsException()
        {
            string foobarString = "testing stuff";
            decimal foobarDecimal = 21;

            try
            {
                var result = _myController.GetStuff(foobarString, foobarDecimal);
            }
            catch (Exception ex)
            {
                return;
            }
            Assert.Fail("Should have thrown an exception.");
        }
```


// C:\Program Files (x86)\Microsoft Visual
Studio\2017\Professional\Common7\IDE\PublicAssemblies\Microsoft.VisualStudio.QualityTools.UnitTestFramework.dll


new

//
C:\Users\peter\.nuget\packages\mstest.testframework\1.3.2\lib\netstandard1.0\Microsoft.VisualStudio.TestPlatform.TestFramework.dll

Assert.ThrowsExceptionAsync

Assert.ThrowsException
