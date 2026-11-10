abstract class MenuItem {

    protected String name;
    protected double basePrice;
    private String itemCode;

    public MenuItem() {
        this.name = "New Cafe Item";
        this.basePrice = 0.0;
        this.itemCode = "NEW-01";
    }

    public MenuItem(String name, double basePrice, String itemCode) {
        this();
        setName(name);
        setBasePrice(basePrice);
        setItemCode(itemCode);
    }

    public abstract double computePrice();

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        }
    }

    public double getBasePrice() {
        return basePrice;
    }

    public void setBasePrice(double basePrice) {
        if (basePrice >= 0) {
            this.basePrice = basePrice;
        } else {
            System.out.println("Invalid price. Price cannot be negative.");
        }
    }

    public String getItemCode() {
        return itemCode;
    }

    public void setItemCode(String itemCode) {
        if (itemCode != null && !itemCode.trim().isEmpty()) {
            this.itemCode = itemCode;
        }
    }
}
public class Pastry extends MenuItem {

    private static final double WARMING_FEE = 15.0;

    private boolean warmed;

    public Pastry() {
        super();
        this.warmed = true;
    }

    public Pastry(String name, double basePrice,
                  String itemCode, boolean warmed) {

        super(name, basePrice, itemCode);
        this.warmed = warmed;
    }

    public boolean isWarmed() {
        return warmed;
    }

    public void setWarmed(boolean warmed) {
        this.warmed = warmed;
    }

    @Override
    public double computePrice() {

        double price = basePrice;

        if (warmed) {
            price += WARMING_FEE;
        }

        return price;
    }
}
class CafeApp {

    public static void main(String[] args) {

        Customer ana = new Customer("Amaro Kayle Nicole", true);
        Customer zeke = new Customer(" Pausal Yhimkie", false);


        Beverage icedLatte = new Beverage(
            "Iced Latte", 95, "B-01", "L"
        );

        Pastry ensaymada = new Pastry(
            "Ensaymada", 49, "P-01", true
        );

        Meal chickenAdobo = new Meal(
            "Chicken Adobo", 110, "M-01", true, 1
        );

        Pastry cookie = new Pastry(
            "Cookie", 30, "P-02", false
        );

        Order order1 = new Order("1001", ana);

        order1.addItem(icedLatte);
        order1.addItem(ensaymada);
        order1.addItem(chickenAdobo);
        order1.addItem(cookie);

        order1.printReceipt();

        System.out.println();


        Beverage hotChocolate = new Beverage();

        hotChocolate.setBasePrice(80);
        hotChocolate.setName("Hot Chocolate");
        hotChocolate.setItemCode("B-02");
        hotChocolate.setSize("M");

        Meal beefTapa = new Meal();

        beefTapa.setName("Beef Tapa");
        beefTapa.setBasePrice(130);
        beefTapa.setItemCode("M-02");

        Order order2 = new Order("1002", zeke);

        order2.addItem(hotChocolate);
        order2.addItem(beefTapa);

        order2.printReceipt();
    }
}
public class Order {

    private static final int MAX_ITEMS = 4;
    private static final double STUDENT_DISCOUNT_RATE = 0.15;

    private Customer customer;

    private MenuItem item1;
    private MenuItem item2;
    private MenuItem item3;
    private MenuItem item4;

    private int itemCount;
    private String orderId;
    private double voucher;

    public Order(String orderId, Customer customer) {
        this.orderId = orderId;
        this.customer = customer;
        this.itemCount = 0;
        this.voucher = 20.0;
    }

    public void setVoucher(double voucher) {
        if (voucher >= 0) {
            this.voucher = voucher;
        } else {
            System.out.println("Invalid voucher amount.");
        }
    }

    public void addItem(MenuItem item) {

        if (item == null) {
            System.out.println("Invalid item.");
            return;
        }

        if (itemCount >= MAX_ITEMS) {
            System.out.println("Order limit reached! Maximum 4 items.");
            return;
        }

        switch (itemCount) {
            case 0:
                item1 = item;
                break;

            case 1:
                item2 = item;
                break;

            case 2:
                item3 = item;
                break;

            case 3:
                item4 = item;
                break;
        }

        itemCount++;

        System.out.println(item.getName() + " added successfully!");
    }

    public double calculateSubtotal() {

        double subtotal = 0;

        if (item1 != null) {
            subtotal += item1.computePrice();
        }

        if (item2 != null) {
            subtotal += item2.computePrice();
        }

        if (item3 != null) {
            subtotal += item3.computePrice();
        }

        if (item4 != null) {
            subtotal += item4.computePrice();
        }

        return subtotal;
    }

    public void printReceipt() {

        double subtotal = calculateSubtotal();

        double discount = 0;

        if (customer.isStudent()) {
            discount = subtotal * STUDENT_DISCOUNT_RATE;
        }

        double total = subtotal - discount - voucher;

        if (total < 0) {
            total = 0;
        }

        System.out.println("========================================================");
        System.out.println("                  CAMPUS CAFE RECEIPT");
        System.out.println("========================================================");

        System.out.println("Order ID: " + orderId);
        System.out.println("Customer: " + customer.getName());

        System.out.println("--------------------------------------------------------");
        System.out.printf("%-30s %12s%n", "ITEM", "PRICE");
        System.out.println("--------------------------------------------------------");

        if (item1 != null) {
            System.out.printf("%-30s %12.2f%n",
                    item1.getName(), item1.computePrice());
        }

        if (item2 != null) {
            System.out.printf("%-30s %12.2f%n",
                    item2.getName(), item2.computePrice());
        }

        if (item3 != null) {
            System.out.printf("%-30s %12.2f%n",
                    item3.getName(), item3.computePrice());
        }

        if (item4 != null) {
            System.out.printf("%-30s %12.2f%n",
                    item4.getName(), item4.computePrice());
        }

        System.out.println("--------------------------------------------------------");

        System.out.printf("%-30s %12.2f%n", "Subtotal:", subtotal);
        System.out.printf("%-30s %12.2f%n", "Student Discount:", discount);
        System.out.printf("%-30s %12.2f%n", "Voucher:", voucher);

        System.out.println("--------------------------------------------------------");

        System.out.printf("%-30s %12.2f%n", "TOTAL:", total);

        customer.addLoyaltyPoints(total);

        System.out.println("Loyalty Points: "
                + customer.getLoyaltyPoints());

        System.out.println("========================================================");
        System.out.println("              Thank you for ordering!");
        System.out.println("========================================================");
    }
}
public class Customer {

    private static final double PESOS_PER_POINT = 40.0;

    private String name;
    private boolean student;
    private int loyaltyPoints;

    public Customer() {
        this.name = "Walk-in Customer";
        this.student = false;
        this.loyaltyPoints = 50;
    }

    public Customer(String name) {
        this();
        setName(name);
    }

    public Customer(String name, boolean student) {
        this();
        setName(name);
        this.student = student;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        }
    }

    public boolean isStudent() {
        return student;
    }

    public void setStudent(boolean student) {
        this.student = student;
    }

    public int getLoyaltyPoints() {
        return loyaltyPoints;
    }

    public void addLoyaltyPoints(double amount) {
        if (amount > 50) {
            loyaltyPoints += (int)(amount / PESOS_PER_POINT);
        }
    }
}
public class Beverage extends MenuItem {

    private String size;

    public Beverage() {
        super();
        this.size = "M";
    }

    public Beverage(String name, double basePrice,
                    String itemCode, String size) {

        super(name, basePrice, itemCode);
        this.size = "M";
        setSize(size);
    }

    public String getSize() {
        return size;
    }

    public void setSize(String size) {
        if (size != null &&
            (size.equals("S") || size.equals("M") ||
             size.equals("L"))) {

            this.size = size;
        } else {
            System.out.println("Invalid beverage size.");
        }
    }

    @Override
    public double computePrice() {

        if (size.equals("L")) {
            return basePrice + 30;
        } else if (size.equals("M")) {
            return basePrice + 20;
        }

        return basePrice;
    }
}
public class Meal extends MenuItem {

    private static final double UPSIZE_FEE = 30.0;
    private static final double SIDE_PRICE = 18.0;

    private boolean upsized;
    private int extraSides;

    public Meal() {
        super();
        this.upsized = true;
        this.extraSides = 1;
    }

    public Meal(String name, double basePrice,
                String itemCode, boolean upsized,
                int extraSides) {

        super(name, basePrice, itemCode);

        this.upsized = upsized;
        this.extraSides = 0;
        setExtraSides(extraSides);
    }

    public boolean isUpsized() {
        return upsized;
    }

    public void setUpsized(boolean upsized) {
        this.upsized = upsized;
    }

    public int getExtraSides() {
        return extraSides;
    }

    public void setExtraSides(int extraSides) {
        if (extraSides >= 0) {
            this.extraSides = extraSides;
        } else {
            System.out.println("Invalid number of extra sides.");
        }
    }

    @Override
    public double computePrice() {

        double price = basePrice;

        if (upsized) {
            price += UPSIZE_FEE;
        }

        price += extraSides * SIDE_PRICE;

        return price;
    }
}
