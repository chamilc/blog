---
title: Xamarin Forms, Use ListView control to display items like a Collection View
---
By [Suthakaran] 

Xamarin apps runs on variety of devices with different screen sizes and app UI should respond to different screen sizes as well as orientation changes. Very often we encounter with scenarios to display a collection of items in apps, for small devices it is ok to use typical Listview to render the collection where each row in the listview represent single item. But displaying single item in each row is not good idea when app runs on bigger devices such as tablets. It would be nice user experience if app display items like a matrix view (Rows and Columns). 

By default, all the native platforms provides some way to display collection of items in whatever the way we expect.iOS provides a collection view controller(UICollectionView)  by default. It is just matter of defining the child view templates and pass the desired  data, Collection View will render the items in nice wrap layout based on the child view template and available space in the screen. See this link for more details about [UICollectionView].Similar behavior can be achieved in Android using [Table Layout] or [Grid Layout].
If we develop the native apps with Xamarin, it is possible to use the UICollectionView in iOS or Table Layout in Android to achieved desired behavior. But when it comes to Xamarin.Forms application, it needs lots of effort to expose these views via custom renderer for each platform. Alternatively Xamarin.Forms provides Layout class which is a specialized subtype of View, which acts as a container for other Layouts or Views. Xamarin.Forms defines four layout classes – StackLayout, AbsoluteLayout, RelativeLayout, and Grid, and each arranges its children in a different way. In Xamarin.Forms, all layout classes derive from the Layout<<T>> class and constrain the generic type to View and its derived types. In turn, the Layout<<T>> class derives from the Layout class, which provides the mechanism for positioning and sizing child elements. It is very easy to create simple wrap layout using Layout class and placing child views in rows and columns based on screen size and child view’s size. 
```C#
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```
Refer this xamarin documentation for more details about [custom layout] implementation.

But implementing the wrap layout ourself might bring performance overhead when it comes to handling large number of items, specially with scrolling. I encountered such performance issue while using the custom made wraplayout in one of the project,but I found an alternative way to display items in rows and column using Xamarin.Forms ListView control with few minor limitations, but it served my purpose.Hope this might help you as well. Only limitation is we have to preconfigure the column count for different screen sizes and define multiple ListView Data Templates for each configuration, but it is worth doing considering the in built features available such as Caching, Recycle and Retaining Element mechanism which is very hard to implement with custom layouts. Refer [this article] for details about the ListView Control.

In this example ,I created a document explorer kind of view using Xamarin Forms ListView control for tablet devices. When device orientation changes, number of items in each row will change based on configured value. It can be extended to different screen sizes easily. 

First define the view model  based on Xamarin Forms recommended MVVM pattern. 
View model directly represent the data to be displayed in the view, in this case each item will be displayed as square frame in rows and columns. Based on device orientation, number of items in each row will change. In order to support such behavior I have defined the view model. You can view the full source code [here].

<code>
public class DocumentModuleViewModel: ViewModelBase
    {
        private ObservableCollection<DocumentViewRowViewModel> documentCollection;
        public ObservableCollection<DocumentViewRowViewModel> DocumentCollection
        {
            get { return documentCollection; }
            set { SetProperty(ref documentCollection, value); }
        }
        public DocumentModuleViewModel(IList<String> documentItems, int rowWidth)
        {
			documentCollection = new ObservableCollection<DocumentViewRowViewModel>();
            int index = 0;
            DocumentViewRowViewModel documentRow = new DocumentViewRowViewModel();
            foreach (var document in documentItems)
            {
                if(index == 0)
                {
                    documentRow = new DocumentViewRowViewModel();
                }
                documentRow.DocumentViewRowCollection.Add( new DocumentItemViewModel() { Name= document});
                index++;
                if(index == rowWidth)
                {
                    documentCollection.Add(documentRow);
                    index = 0;
                }                
            }
			if (index != 0 && documentRow.DocumentViewRowCollection.Count > 0)
			{
				int dummyCount = rowWidth - documentRow.DocumentViewRowCollection.Count;
				for (int i = 0; i < dummyCount; i++)
				{
					documentRow.DocumentViewRowCollection.Add(new DocumentItemViewModel() { IsNotDummy = false });
				}
				documentCollection.Add(documentRow);
			}
        }
    }
    </code>

Then in the view, define data templates for each orientation, in this case one for landscape and other one for portrait. In this example I have considered only tablets with general size. If we have to accommodate many different screen sizes, we might have to adjust the child view size or create different data template.

```xml
	<DataTemplate x:Key="DocViewTabLandscape">
		<ViewCell>
			<Grid Margin="4,10,4,10">
				 <Grid.RowDefinitions>
					<RowDefinition Height="*"></RowDefinition>
				</Grid.RowDefinitions>
				<Grid.ColumnDefinitions>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
				</Grid.ColumnDefinitions>

				<Frame WidthRequest="60" HeightRequest="60" HasShadow="true" Grid.Row ="0" Grid.Column="0" IsVisible="{Binding DocumentViewRowCollection[0].IsNotDummy}">
					<StackLayout>
						<BoxView WidthRequest="36" HeightRequest="36" BackgroundColor="#00CCFF"></BoxView>
						<Label Text= "{Binding DocumentViewRowCollection[0].Name}" Style="{x:StaticResource labelTest}"/>
					</StackLayout>
				</Frame>
			--------------------------------
			</Grid>
		</ViewCell>
	</DataTemplate>
```

```xml
	<DataTemplate x:Key="DocViewTabPotrait">
		<ViewCell>
			<Grid Margin="4,10,4,10">
				 <Grid.RowDefinitions>
					<RowDefinition Height="*"></RowDefinition>
				</Grid.RowDefinitions>
				<Grid.ColumnDefinitions>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
					<ColumnDefinition Width="*"></ColumnDefinition>
				</Grid.ColumnDefinitions>

				<Frame WidthRequest="60" HeightRequest="60" HasShadow="true" Grid.Row ="0" Grid.Column="0" IsVisible="{Binding DocumentViewRowCollection[0].IsNotDummy}">
					<StackLayout>
						<BoxView WidthRequest="36" HeightRequest="36" BackgroundColor="#00CCFF"></BoxView>
						<Label Text= "{Binding DocumentViewRowCollection[0].Name}" Style="{x:StaticResource labelTest}"/>
					</StackLayout>
				</Frame>
			--------------------------------------
			</Grid>
		</ViewCell>
	</DataTemplate>
```

Once the data templates are defined, let the listview to choose the template based on given condition, in this case, device orientation. Selection of template is wired through a DataTemplateSelector provided by Xamarin Forms.

```xml
<ListView x:Name="documentListView" ItemTemplate="{StaticResource documentViewTemplateSelector}"  >
	</ListView>

```

Data Template Selector 

```C#
	public class DocumentViewTemplateSelector : DataTemplateSelector
	{
		public static bool IsLandscapeMode = false;

		public DataTemplate LandscapeTemplate { get; set; }
		public DataTemplate PortriatTemplate { get; set; }

		protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
		{
			return IsLandscapeMode ? LandscapeTemplate : PortriatTemplate;
		}
	}
```

In the device orientation changed event handler, prepare the viewmodel to support the selected Data template. 

```C#
		void Handle_SizeChanged(object sender, System.EventArgs e)
		{
			if (Width > Height)
				DocumentViewTemplateSelector.IsLandscapeMode = true;
			else
				DocumentViewTemplateSelector.IsLandscapeMode = false;

			PrepareDocumentViewData(DocumentViewTemplateSelector.IsLandscapeMode);
		}

```
As our app needed a document explorer, using listview with multiple data template was a good choice to provide different views like any other file explorer with options like details view, small icon, content and Icon view etc. 

You might have noticed, there are some dummy items added to the view model which is necessary to represent each row in the list view. Each row expects predefined number of items, usually last row might have less number of items than needed. To fill the row, we add those dummy items which will not be rendered. It is handled in view definition.  

```xml
<Frame WidthRequest="60" HeightRequest="60" HasShadow="true" Grid.Row ="0" Grid.Column="0" IsVisible="{Binding DocumentViewRowCollection[0].IsNotDummy}">
```

Using listview this way is useful when we need to display items in rows and columns, but Layout<T> class more flexible than Listview, Layout class can be used to arrange the collections in any shape such as circle or in a curve etc...

References: <br/>
1.<https://developer.xamarin.com/guides/xamarin-forms/user-interface/layouts/custom> <br/>
2.<https://developer.xamarin.com/guides/xamarin-forms/user-interface/listview/data-and-databinding/><br/>
3.<https://developer.xamarin.com/guides/xamarin-forms/application-fundamentals/templates/data-templates/selector/>



[//]: #comments 
   [Suthakaran]:<https://www.linkedin.com/in/suthakaransivabalan/>
   [UICollectionView]:<https://developer.apple.com/documentation/uikit/uicollectionview>
   [Table Layout]:<https://developer.android.com/reference/android/widget/TableLayout.html>
   [Grid Layout]:<https://developer.android.com/reference/android/widget/GridLayout.html>
   [custom layout]:<https://developer.xamarin.com/guides/xamarin-forms/user-interface/layouts/custom/>
   [this article]:<https://developer.xamarin.com/guides/xamarin-forms/user-interface/listview/performance>
   [here]:<https://github.com/GeveoAu/BlogRepos/tree/master/MarixView>
   
  

   


   
