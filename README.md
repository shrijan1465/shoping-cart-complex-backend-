#include<string>
#include<unordered_map>
using namespace std;

class Item;
class Cart;

class Product{
int id;
string name;
int price;

public:
    Product (){

    }
    Product(int u_id, string name, int price){
        this->name=name;
        this->price=price;
    }
    string getDisplayName(){
        return name + " : RS " + to_string(price) + "\n";
    }
    string getShortName(){
        return name.substr(0,1);
    }
    friend class  Item;
    friend class Cart;
};

class Item{
    Product product;
    int quantity;

    public:
    Item(){

    }
    Item(Product p, int q):product(p), quantity(q){}

    int getItemPrice(){
        return quantity*product.price;
    }
    string getItemInfo(){
        return to_string(quantity) + " x " +product.name + "RS. "+ to_string(quantity*product.price) + "\n";
    }
    friend class Cart;

};

class Cart{
    unordered_map<int,Item> items;
public:

    void addProduct (Product product){
    if(items.count(product.id)==0){
    Item newItem(product,1);
    items [product.id] = newItem;
    }

else{
    items [product.id].quantity += 1;
    }
}

    int getTotal(){
       int total= 0;
         for (auto itemPair : items ){
            auto item = itemPair.second;
            total+=item.getItemPrice();
         }
    }
    string viewCart(){
    if (items.empty()){
        return "Cart is empty";
}

string itemizedList;
    int cart_total = getTotal();
    for(auto itemPair:items){
        auto item = itemPair.second;
        itemizedList.append(item.getItemInfo());
    }
    return itemizedList + "\n Total amount : RS." + to_string(cart_total)+'\n';
}

    bool isEmpity(){
        return items.empty();
    }
};


#include<iostream>
#include<vector>
using namespace std;

vector<Product> allProducts = {

Product(1, "apple", 26),

Product(3,"mango", 16),

Product(2,"guava", 36),

Product(5, "banana", 56),

Product(4,"strawberry", 29),

Product(6,"pineapple", 20),

};

Product* chooseProduct(){

//Display the list of products

string productList;

cout<<"Available Products "<<endl;

for(auto product : allProducts) {
         cout << product.getDisplayName();
     //productList.append(product.getDisplayName());
}
cout<<"----------------------------"<<endl;

string choice;
cin>>choice;
for (int i=0; i<allProducts.size(); i++){
    if(allProducts[i].getShortName()==choice){
        return &allProducts[i];
    }

}
cout<<"Item not found try latter"<<endl;
return NULL;
}

int main (){

char action ;
Cart cart;

while (true){
    cout<<"select an action - (a)dd item, (v)iew cart, (C)heckout"<<endl;
    cin>> action;

    if (action== 'a'){
        Product*product=chooseProduct();
        if(product!=NULL){
            cout<<"add to the cart "<<product->getDisplayName()<<endl;
            cart.addProduct(*product);
        }
        else if (action=='v'){
            //view the cart 
            cout<<"-----------------------"<<endl;
            cout<<cart.viewCart();
            cout<<"-----------------------"<<endl;
        }
        else if (action == 'C' || action == 'c') {
            cout << "Checking out...\n";
            cout << cart.viewCart();
            break;
        }
        else{
             cout << "Invalid action! Try again." << endl;

        }
    }
    return 0;
}

}
