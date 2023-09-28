# how-to-automatically-scroll-to-bring-the-selected-item-into-the-view-in-.net-maui-listView

This example shows how to automatically scroll to bring the selected item into the view in .NET MAUI ListView.
 
## C#

public class ListViewBehavior : Behavior<SfListView>
{
    private SfListView ListView;
 
    protected override void OnAttachedTo(SfListView bindable)
    {
        ListView = bindable;
        ...
 
        ListView.PropertyChanged += ListView_PropertyChanged;
        base.OnAttachedTo(bindable);
    }
 
    private void ListView_PropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
    {
        if (e.PropertyName == "SelectedItem")
        {
            ListView.Dispatcher.Dispatch(async () =>
            {
                await Task.Delay(100);
                if (this.ListView.DataSource.DisplayItems.Count > 0)
                {
                    var selectedItemIndex = ListView.DataSource.DisplayItems.IndexOf(ListView.SelectedItem);
                    selectedItemIndex += (ListView.HeaderTemplate != null && !ListView.IsStickyHeader && !ListView.IsStickyGroupHeader) ? 1 : 0;
                    (ListView.ItemsLayout as LinearLayout).ScrollToRowIndex(selectedItemIndex);
                }
            });
        }
    }
}


## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.