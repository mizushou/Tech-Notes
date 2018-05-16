### TableView
---
- https://docs.oracle.com/javase/jp/8/javafx/api/javafx/scene/control/TableColumn.html

#### TableColumn<S, T>
---
  - S : The first one is just type of data in your table.
  - T : The second one is going to be in this column.
    - ex) Product table's name column -> Product.Name
    ```
    TableColumn<Product, String> nameColumn = new TableColumn<>("Name");
    ```
  - Two values are important to make TableColumn instance.
    1. Header' name
    2. Cell value factory (it's callback that returns ObservableValue)
      ```
      //Name cloumn
      TableColumn<Product, String> nameColumn = new TableColumn<>("Name"); // header
      nameColumn.setMinWidth(200);
      nameColumn.setCellValueFactory(new PropertyValueFactory<>("name")); // cell value factory
      ```
  - setCellValueFactory
    - You need set CellValueFactory evne if you use FXML and Controller.
    - The setCellValueFactory method specifies a **cell factory** for each column.
      - https://docs.oracle.com/javafx/2/ui_controls/table-view.htm
      - https://stackoverflow.com/questions/27806367/cant-populate-javafx-tableview-created-in-scenebuilder-2

- How to display data
  1. Create an ObservatablList array and define as many data rows as you would like to show in your table.
    ```
    public ObservableList<Product> getProduct() {
        ObservableList<Product> products = FXCollections.observableArrayList();
        products.add(new Product("Laptop", 859.00, 20));
        products.add(new Product("Bouncy Ball", 2.49, 198));
        products.add(new Product("Toilet", 99.00, 74));
        products.add(new Product("Bouncy Ball", 2.49, 198));
        products.add(new Product("The NoteBook DVD", 19.99, 198));
        products.add(new Product("Corn", 1.49, 856));
        return products;
    }
    ```
  2. Associate the data with the table columns.
    ```
    //Name cloumn
    TableColumn<Product, String> nameColumn = new TableColumn<>("Name"); // header
    nameColumn.setMinWidth(200);
    nameColumn.setCellValueFactory(new PropertyValueFactory<>("name"));

    //Price cloumn
    TableColumn<Product, Double> priceColumn = new TableColumn<>("Price"); // header
    priceColumn.setMinWidth(100);
    priceColumn.setCellValueFactory(new PropertyValueFactory<>("price"));

    //Quantity cloumn
    TableColumn<Product, Integer> quantityColumn = new TableColumn<>("Quantity"); // header
    quantityColumn.setMinWidth(100);
    quantityColumn.setCellValueFactory(new PropertyValueFactory<>("quantity"));
    ```
  3. Add the data to the table by using the **setItems** method of the TableView class.
    ```
    table.setItems(getProduct());
    ```
