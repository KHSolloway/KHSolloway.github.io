### Kristina Solloway
### NikeStore.ERD.md

# Nike Shoe Store Ordering Process

```mermaid
---
config:
  theme: forest
---

erDiagram
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER { 
        int CustID PK
        string FirstName
        string LastName
        string Email
        string Phone UK
        string Address
        string City
        string State
        string Zipcode
    }
    CUSTOMER ||--o{ PAYMENT : "customer <br /> selects"
    PAYMENT {
        int PaymentID UK
        string CreditCardType "Mastercard, VISA, AMEX"
        string CCFirstName "Name on card"
        string CCLastName
        string CCAddress
        string CCCity
        string CCState
        string CCZipcode
        string CCAccountNumber
        string CCcvc
        date CCExpirationDate
    }
    CUSTOMER ||--|{ DELIVERY-ADDRESS : "customer <br /> selects"
    DELIVERY-ADDRESS {
        int DlvryID PK "ship to address can be different from customer address"
        int CustID FK
        string DlvryFirstName 
        string DlvryLastName
        string DlvryEmail
        string DlvryPhone
        string DlvryAddress
        string DlvryCity
        string DlvryState
        string DlvryZipcode
    }
    ORDER ||--|{ LINE-ITEM : contains
    ORDER ||--|| DELIVERY-ADDRESS : "ships to"
    ORDER ||--|| PAYMENT : "charged to <br /> account"
    ORDER {
        int OrderID PK, UK "same as Order Number"
        int CustID FK "links to customer record"
        int DlvryID FK "links to delivery address"
        int PaymentID FK "links to credit card"
    }
    LINE-ITEM ||--|| PRODUCT : "add to cart"
    LINE-ITEM {
        int LineItemID PK
        int OrderID FK
        int ProductID FK
        string Size "ie. Mens Size 12, Ladies Size 8, Childrens Size 2"
        int OrderQuantity "enter desired quantity"
        float PricePerUnit
    }
    PRODUCT ||--|| INVENTORY : "compares order <br /> to available <br /> inventory"
    PRODUCT {
        int ProductID PK
        int ModelNumber UK "item can be searched by its model number"
        int UPC UK "item can be searched by its UPC Code"
        string ProductDescription
        string Color "item can be searched by color"
        string Size "item can be searched by size"
        float PricePerUnit
    }
    INVENTORY ||--|| LINE-ITEM : "deducts from <br /> available <br /> inventory"
    INVENTORY {
        int ProductID FK
        string ProductDescription
        string Size
        int QuantityInStock "indicates available quantity of product"
        float CostPerUnit "Nike's cost"
        float PricePerUnit "sale price to customer"
    }

```
### Description of Entities (database tables)  
**CUSTOMER**  - This table houses the customer's personal contact information; the originator of the ORDER.  
**DELIVERY-ADDRESS** - CUSTOMER can select from a collection of saved delivery addresses, be it to themselves, or to a third party.  
**PAYMENT** - CUSTOMER can select from a collection of on file credit cards accounts for online shopping.  
**ORDER** - CUSTOMER creates a new ORDER by selecting a PRODUCT to add to the cart.  
**PRODUCT** - CUSTOMER searches the PRODUCT table according to personal specifications (ie. size, color, model number, UPC Code).  
PRODUCT table houses all of the products the store offers, and is added to the LINE-ITEM when the CUSOMTER selcts it.  
**LINE-ITEM** - PRODUCT is added to the LINE-ITEM of the ORDER. Multiple LINE-ITEMS can be selected.  
**INVENTORY** - When the PRODUCT is selected for the LINE-ITEM in the ORDER, it is compared to the INVENTORY table to see if the item is in stock.  If it's in stock, the Quantity will be deducted from the INVENTORY table when the sale is finalized. This maintains the available inventory quantities in real time.

