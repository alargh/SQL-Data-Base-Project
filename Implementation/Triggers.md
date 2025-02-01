# Triggers

#### 1. Checking if the full amount has been paid, changing order_paid to 1.

```sql
CREATE TRIGGER trg_After_Payment
ON Payments
AFTER INSERT
AS
BEGIN
    SET NOCOUNT ON;

    BEGIN TRY
        BEGIN TRANSACTION;

        DECLARE @OrderID INT, @ProductID INT, @InsertedPayment MONEY, @Price MONEY, @Deferred BIT;

        SELECT @OrderID = order_id,
               @ProductID = product_id,
               @InsertedPayment = payment_amount
        FROM inserted;

        SELECT @Price = price,
               @Deferred = deferred
        FROM Order_Details
        WHERE order_id = @OrderID AND product_id = @ProductID;

        IF @Deferred = 1
        BEGIN
            DECLARE @TotalPayment MONEY;
            SELECT @TotalPayment = SUM(payment_amount)
            FROM Payments
            WHERE order_id = @OrderID AND product_id = @ProductID;

            IF @TotalPayment >= @Price
            BEGIN
                UPDATE Order_Details
                SET order_paid = 1, payment_date = GETDATE()
                WHERE order_id = @OrderID AND product_id = @ProductID;
            END
        END
        ELSE
        BEGIN
            IF @InsertedPayment >= @Price
            BEGIN
                UPDATE Order_Details
                SET order_paid = 1, payment_date = GETDATE()
                WHERE order_id = @OrderID AND product_id = @ProductID;
            END
        END

        COMMIT TRANSACTION;

    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        THROW;
    END CATCH
END;
GO
```
