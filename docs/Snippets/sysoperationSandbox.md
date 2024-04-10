# SysOperation Sandbox

**Long Processing Dialog  D365FO** [Original Microsoft Community Post](https://community.dynamics.com/blogs/post/?postid=807d999a-906a-4c5e-a748-061aa9af833c)

There is a class in Dynamics 365FO Named SysOperationSandbox which is handle long running dialog for long running processes.
This class support **table and class level static method only**.

Following are the Parameters:

* Class / Table name 
* Method Name
* Container
* Caption Message
* Completion Message
* Failure Message 

!!!warning
    It seems like that passing and returning of TempDB tables is not working, as they are passed through different sessions. (2024-01-11)

## Form Code

```xpp
[Form]
public class SLD_DemoForm extends FormRun
{
    [Control("Button")]
    class FormButtonControl1
    {
        /// <summary>
        ///
        /// </summary>
        public void clicked()
        {
            container _container=["You can pass parameter to your method",1000];
            SysOperationSandbox::callStaticMethod(classNum(SLD_DemoInstance),
            staticMethodStr(SLD_DemoInstance,LongProcess),_container,
            'waiting caption should be here', 'Operation completed message should be here','Operation Cancelled should be here');
            super();
        }

    }

}
```


## Class Code

```xpp
class SLD_DemoInstance
{
    public static void LongProcess(container _container)
    {
        int contervalue= conPeek(_container,2);

        for (int i=1;i <= contervalue;i++)
        {
            /// Your logic should here
            ///
        }

    }

}
```

