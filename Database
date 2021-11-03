--drop table
DROP TABLE carts_detail;
DROP TABLE cod;
DROP TABLE e_wallet;
DROP TABLE others;
DROP TABLE ship_dates;
DROP TABLE shippings;
DROP TABLE order_items_histories;
DROP TABLE order_items;
DROP TABLE inventory_items;
DROP TABLE orders;
DROP TABLE shopping_carts;
DROP TABLE addresses;
DROP TABLE payments;
DROP TABLE users;

-- predefined type, no DDL - MDSYS.SDO_GEOMETRY

-- predefined type, no DDL - XMLTYPE

CREATE TABLE addresses (
    address_id       CHAR(5 CHAR) NOT NULL,
    address_name     VARCHAR2(4000) NOT NULL,
    address_city     VARCHAR2(4000) NOT NULL,
    address_province VARCHAR2(4000) NOT NULL,
    address_zip      INTEGER NOT NULL,
    users_user_id    CHAR(11) NOT NULL
);

ALTER TABLE addresses ADD CONSTRAINT addresses_pk PRIMARY KEY ( address_id );

CREATE TABLE carts_detail (
    inventory_items_inventory_id CHAR(7 CHAR) NOT NULL,
    shopping_carts_cart_id       CHAR(10 CHAR) NOT NULL,
    cart_detail_state            VARCHAR2(4000) NOT NULL,
    cart_detail_timestamp        DATE NOT NULL,
    cart_detail_qty              NUMBER NOT NULL
);

ALTER TABLE carts_detail ADD CONSTRAINT carts_detail_pk PRIMARY KEY ( inventory_items_inventory_id,
                                                                     shopping_carts_cart_id );

CREATE TABLE cod (
    payment_id           CHAR(15 CHAR) NOT NULL,
    addresses_address_id CHAR(5 CHAR) NOT NULL
);

ALTER TABLE cod ADD CONSTRAINT cod_pk PRIMARY KEY ( payment_id );

CREATE TABLE e_wallet (
    payment_id     CHAR(15 CHAR) NOT NULL,
    users_user_id  CHAR(11) NOT NULL,
    ew_number      INTEGER NOT NULL,
    ew_holder_name VARCHAR2(4000) NOT NULL
);

ALTER TABLE e_wallet ADD CONSTRAINT e_wallet_pk PRIMARY KEY ( payment_id );

ALTER TABLE e_wallet ADD CONSTRAINT e_wallet_pkv1 UNIQUE ( ew_number );

CREATE TABLE inventory_items (
    inventory_id             CHAR(7 CHAR) NOT NULL,
    inventory_item_title     VARCHAR2(20) NOT NULL,
    inventory_item_price     NUMBER NOT NULL,
    inventory_item_timestamp DATE NOT NULL,
    inventory_item_qty       NUMBER NOT NULL
);

ALTER TABLE inventory_items ADD CONSTRAINT inventory_items_pk PRIMARY KEY ( inventory_id );

CREATE TABLE order_items (
    inventory_items_inventory_id CHAR(7 CHAR) NOT NULL,
    order_item_code              NUMBER NOT NULL,
    order_item_qty               NUMBER NOT NULL,
    order_item_name              VARCHAR2(4000) NOT NULL,
    orders_order_id              CHAR(5) NOT NULL
);

ALTER TABLE order_items ADD CONSTRAINT order_items_pk PRIMARY KEY ( order_item_code );

CREATE TABLE order_items_histories (
    order_items_order_item_code NUMBER NOT NULL,
    history_sequence            NUMBER NOT NULL,
    history_qty                 NUMBER NOT NULL
);

ALTER TABLE order_items_histories ADD CONSTRAINT o_item_histories_pk PRIMARY KEY ( history_sequence );

CREATE TABLE orders (
    users_user_id             CHAR(11) NOT NULL,
    payment_payment_id        CHAR(15 CHAR) NOT NULL,
    order_id                  CHAR(5) NOT NULL,
    order_date                DATE NOT NULL,
    total_price               NUMBER NOT NULL,
    order_state               VARCHAR2(4000) NOT NULL,
    discount                  FLOAT,
    shippings_shipping_number NUMBER NOT NULL,
    shopping_carts_cart_id    CHAR(10 CHAR) NOT NULL,
    payment_id                CHAR(15 CHAR) NOT NULL
);

CREATE UNIQUE INDEX orders_idx ON
    orders (
        shippings_shipping_number
    ASC );

ALTER TABLE orders ADD CONSTRAINT orders_pk PRIMARY KEY ( order_id );

CREATE TABLE others (
    payment_id CHAR(15 CHAR) NOT NULL
);

ALTER TABLE others ADD CONSTRAINT others_pk PRIMARY KEY ( payment_id );

CREATE TABLE payments (
    payment_id        CHAR(15 CHAR) NOT NULL,
    payment_state     VARCHAR2(20) NOT NULL,
    payment_timestamp DATE NOT NULL
);

ALTER TABLE payments ADD CONSTRAINT payments_pk PRIMARY KEY ( payment_id );

CREATE TABLE ship_dates (
    ship_date                 DATE NOT NULL,
    delivery_state            VARCHAR2(4000) NOT NULL,
    shippings_shipping_number NUMBER NOT NULL
);

ALTER TABLE ship_dates ADD CONSTRAINT ship_dates_pk PRIMARY KEY ( shippings_shipping_number );

CREATE TABLE shippings (
    orders_order_id      CHAR (5 CHAR) NOT NULL,
    shipping_number      NUMBER NOT NULL,
    ship_method          VARCHAR2(4000) NOT NULL,
    ship_charge          NUMBER NOT NULL,
    shipping_state       VARCHAR2(4000) NOT NULL,
    addresses_address_id CHAR(5 CHAR) NOT NULL
);

CREATE UNIQUE INDEX shippings_idx ON
    shippings (
        orders_order_id
    ASC );

ALTER TABLE shippings ADD CONSTRAINT shippings_pk PRIMARY KEY ( shipping_number );



CREATE TABLE shopping_carts (
    cart_id       CHAR(10 CHAR) NOT NULL,
    active        DATE NOT NULL,
    expire_on     DATE NOT NULL,
    users_user_id CHAR(11) NOT NULL
);

ALTER TABLE shopping_carts ADD CONSTRAINT shopping_carts_pk PRIMARY KEY ( cart_id );

CREATE TABLE users (
    user_id         CHAR(11) NOT NULL,
    user_email      VARCHAR2(30) NOT NULL,
    user_password   VARCHAR2(20 CHAR) NOT NULL,
    user_timestamp  DATE NOT NULL,
    user_first_name VARCHAR2(20),
    user_last_name  VARCHAR2(20),
    user_birthdate  DATE
);

ALTER TABLE users ADD CONSTRAINT users_pk PRIMARY KEY ( user_id );

ALTER TABLE addresses
    ADD CONSTRAINT addresses_users_fk FOREIGN KEY ( users_user_id )
        REFERENCES users ( user_id );

--  ERROR: FK name length exceeds maximum allowed length(30) 
ALTER TABLE carts_detail
    ADD CONSTRAINT c_detail_inventory_items_fk FOREIGN KEY ( inventory_items_inventory_id )
        REFERENCES inventory_items ( inventory_id );

ALTER TABLE carts_detail
    ADD CONSTRAINT carts_detail_shopping_carts_fk FOREIGN KEY ( shopping_carts_cart_id )
        REFERENCES shopping_carts ( cart_id );

ALTER TABLE cod
    ADD CONSTRAINT cod_addresses_fk FOREIGN KEY ( addresses_address_id )
        REFERENCES addresses ( address_id );

ALTER TABLE cod
    ADD CONSTRAINT cod_payment_fk FOREIGN KEY ( payment_id )
        REFERENCES payments ( payment_id );

ALTER TABLE e_wallet
    ADD CONSTRAINT e_wallet_payment_fk FOREIGN KEY ( payment_id )
        REFERENCES payments ( payment_id );

ALTER TABLE e_wallet
    ADD CONSTRAINT e_wallet_users_fk FOREIGN KEY ( users_user_id )
        REFERENCES users ( user_id );

--  ERROR: FK name length exceeds maximum allowed length(30) 
ALTER TABLE order_items_histories
    ADD CONSTRAINT o_items_histories_o_items_fk FOREIGN KEY ( order_items_order_item_code )
        REFERENCES order_items ( order_item_code );

ALTER TABLE order_items
    ADD CONSTRAINT order_items_inventory_items_fk FOREIGN KEY ( inventory_items_inventory_id )
        REFERENCES inventory_items ( inventory_id );

ALTER TABLE order_items
    ADD CONSTRAINT order_items_orders_fk FOREIGN KEY ( orders_order_id )
        REFERENCES orders ( order_id );

ALTER TABLE orders
    ADD CONSTRAINT orders_payments_fk FOREIGN KEY ( payment_payment_id )
        REFERENCES payments ( payment_id );

ALTER TABLE orders
    ADD CONSTRAINT orders_shopping_carts_fk FOREIGN KEY ( shopping_carts_cart_id )
        REFERENCES shopping_carts ( cart_id );

ALTER TABLE orders
    ADD CONSTRAINT orders_users_fk FOREIGN KEY ( users_user_id )
        REFERENCES users ( user_id );

ALTER TABLE others
    ADD CONSTRAINT others_payment_fk FOREIGN KEY ( payment_id )
        REFERENCES payments ( payment_id );

ALTER TABLE ship_dates
    ADD CONSTRAINT ship_dates_shippings_fk FOREIGN KEY ( shippings_shipping_number )
        REFERENCES shippings ( shipping_number );

ALTER TABLE shippings
    ADD CONSTRAINT shippings_addresses_fk FOREIGN KEY ( addresses_address_id )
        REFERENCES addresses ( address_id );

ALTER TABLE shopping_carts
    ADD CONSTRAINT shopping_carts_users_fk FOREIGN KEY ( users_user_id )
        REFERENCES users ( user_id );

CREATE OR REPLACE TRIGGER fkntm_orders BEFORE
    UPDATE OF users_user_id ON orders
BEGIN
    raise_application_error(-20225, 'Non Transferable FK constraint  on table ORDERS is violated');
END;
/

CREATE OR REPLACE TRIGGER fkntm_shopping_carts BEFORE
    UPDATE OF users_user_id ON shopping_carts
BEGIN
    raise_application_error(-20225, 'Non Transferable FK constraint  on table SHOPPING_CARTS is violated');
END;
/

CREATE OR REPLACE TRIGGER arc_fkarc_1_cod BEFORE
    INSERT OR UPDATE OF payment_id ON cod
    FOR EACH ROW
DECLARE
    d CHAR(15 CHAR);
BEGIN
    SELECT
        a.payment_id
    INTO d
    FROM
        payments a
    WHERE
        a.payment_id = :new.payment_id;

    IF ( d IS NULL OR d <> 'not null' ) THEN
        raise_application_error(-20223,
                               'FK COD_PAYMENT_FK in Table COD violates Arc constraint on Table PAYMENTS - discriminator column payment_id doesn''t have value ''not null''');
    END IF;

EXCEPTION
    WHEN no_data_found THEN
        NULL;
    WHEN OTHERS THEN
        RAISE;
END;
/

CREATE OR REPLACE TRIGGER arc_fkarc_1_e_wallet BEFORE
    INSERT OR UPDATE OF payment_id ON e_wallet
    FOR EACH ROW
DECLARE
    d CHAR(15 CHAR);
BEGIN
    SELECT
        a.payment_id
    INTO d
    FROM
        payments a
    WHERE
        a.payment_id = :new.payment_id;

    IF ( d IS NULL OR d <> 'not null' ) THEN
        raise_application_error(-20223,
                               'FK E_WALLET_PAYMENT_FK in Table E_WALLET violates Arc constraint on Table PAYMENTS - discriminator column payment_id doesn''t have value ''not null''');
    END IF;

EXCEPTION
    WHEN no_data_found THEN
        NULL;
    WHEN OTHERS THEN
        RAISE;
END;
/

CREATE OR REPLACE TRIGGER arc_fkarc_1_others BEFORE
    INSERT OR UPDATE OF payment_id ON others
    FOR EACH ROW
DECLARE
    d CHAR(15 CHAR);
BEGIN
    SELECT
        a.payment_id
    INTO d
    FROM
        payments a
    WHERE
        a.payment_id = :new.payment_id;

    IF ( d IS NULL OR d <> 'not null' ) THEN
        raise_application_error(-20223,
                               'FK OTHER_PAYMENT_FK in Table OTHERS violates Arc constraint on Table PAYMENTS - discriminator column payment_id doesn''t have value ''not null''');
    END IF;

EXCEPTION
    WHEN no_data_found THEN
        NULL;
    WHEN OTHERS THEN
        RAISE;
END;
/

--Users

INSERT INTO users (user_id, user_email, user_password, user_timestamp, user_first_name, user_last_name, user_birthdate) 
Values ('ECU12345678', 'mingguri52@gmail.com', 'hahaha1234', TO_DATE('2020-07-11', 'yyyy-mm-dd'), 'Kim', 'Minju', TO_DATE('2001-02-05', 'yyyy-mm-dd'));
INSERT INTO users (user_id, user_email, user_password, user_timestamp, user_first_name, user_last_name, user_birthdate) 
Values ('HNB25129988', 'hadyanbachdim@gmail.com', 'dededidi00', TO_DATE('2020-09-06', 'yyyy-mm-dd'), 'Hadyan', 'Bachdim', TO_DATE('2001-01-01', 'yyyy-mm-dd'));
INSERT INTO users (user_id, user_email, user_password, user_timestamp, user_first_name, user_last_name, user_birthdate) 
Values ('JAE34567898', 'jaeminna138@gmail.com', 'halohalo8', TO_DATE('2020-09-06', 'yyyy-mm-dd'), 'Na', 'Jaemin', TO_DATE('2000-08-13', 'yyyy-mm-dd'));
INSERT INTO users (user_id, user_email, user_password, user_timestamp, user_first_name, user_last_name, user_birthdate) 
Values ('RFT23890122', 'realfatih@gmail.com', 'temansebaya23', TO_DATE('2020-11-08', 'yyyy-mm-dd'), 'Fatih', 'Siregar', TO_DATE('2002-06-10', 'yyyy-mm-dd'));

--Shopping Carts

INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC12345678', TO_DATE('2021-04-28', 'yyyy-mm-dd'), TO_DATE('2021-05-05', 'yyyy-mm-dd'), 'ECU12345678');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC12345674', TO_DATE('2021-05-20', 'yyyy-mm-dd'), TO_DATE('2021-05-27', 'yyyy-mm-dd'), 'ECU12345678');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC12345423', TO_DATE('2021-05-17', 'yyyy-mm-dd'), TO_DATE('2021-05-24', 'yyyy-mm-dd'), 'HNB25129988');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC42315428', TO_DATE('2021-05-28', 'yyyy-mm-dd'), TO_DATE('2021-06-04', 'yyyy-mm-dd'), 'HNB25129988');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC31925536', TO_DATE('2021-05-22', 'yyyy-mm-dd'), TO_DATE('2021-05-29', 'yyyy-mm-dd'), 'HNB25129988');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC12345612', TO_DATE('2021-05-22', 'yyyy-mm-dd'), TO_DATE('2021-05-29', 'yyyy-mm-dd'), 'JAE34567898');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC23856217', TO_DATE('2021-12-21', 'yyyy-mm-dd'), TO_DATE('2021-12-28', 'yyyy-mm-dd'), 'RFT23890122');
INSERT INTO shopping_carts (cart_id, active, expire_on, users_user_id)
Values ( 'SC23898721', TO_DATE('2021-12-23', 'yyyy-mm-dd'), TO_DATE('2021-12-30', 'yyyy-mm-dd'), 'RFT23890122');

--payments

INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA32645673827BC', 'Success', TO_DATE('2021-05-01', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA36274825473BC', 'Success', TO_DATE('2021-05-24', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA36274134562BC', 'Success', TO_DATE('2021-05-19', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA36274345663BC', 'Success', TO_DATE('2021-05-30', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA23456778765BC', 'Failed', TO_DATE('2021-05-30', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA56723189039BC', 'Success', TO_DATE('2021-12-23', 'yyyy-mm-dd'));
INSERT INTO payments(payment_id, payment_state, payment_timestamp)
Values ('PA56723234239BC', 'Success', TO_DATE('2021-12-28', 'yyyy-mm-dd'));

--Orders

INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, discount, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('ECU12345678', 'PA32645673827BC', 'O501B', TO_DATE('2021-05-01', 'yyyy-mm-dd'), 154000, 'Completed', 20/100, 54637, 'SC12345678',  'PA32645673827BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('ECU12345678', 'PA36274825473BC', 'O502B', TO_DATE('2021-05-24', 'yyyy-mm-dd'), 20000, 'Completed', 12432, 'SC12345674',  'PA36274825473BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('HNB25129988', 'PA36274134562BC', 'O503B', TO_DATE('2021-05-19', 'yyyy-mm-dd'), 299000, 'Completed', 13579, 'SC12345423',  'PA36274134562BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, discount, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('HNB25129988', 'PA36274345663BC', 'O504B', TO_DATE('2021-05-30', 'yyyy-mm-dd'), 850000, 'Completed', 15/100, 12490, 'SC42315428',  'PA36274345663BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, discount, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('JAE34567898', 'PA23456778765BC', 'O505B', TO_DATE('2021-05-24', 'yyyy-mm-dd'), 50000, 'Canceled', 10/100, 15242, 'SC12345612',  'PA23456778765BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('RFT23890122', 'PA56723189039BC', 'O506B', TO_DATE('2021-12-23', 'yyyy-mm-dd'), 25000, 'Completed', 14625, 'SC23856217',  'PA56723189039BC');
INSERT INTO orders (users_user_id, payment_payment_id, order_id, order_date, total_price, order_state, discount, shippings_shipping_number, shopping_carts_cart_id, payment_id)
Values ('RFT23890122', 'PA56723234239BC', 'O507B', TO_DATE('2021-12-28', 'yyyy-mm-dd'), 115000, 'Completed', 10/100, 17008, 'SC23898721',  'PA56723234239BC');


--Inventory Items

INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1234', 'Notes', 25000, TO_DATE('2021-03-01', 'yyyy-mm-dd'), 30);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1235', 'Tumbler', 54000, TO_DATE('2021-04-30', 'yyyy-mm-dd'), 15);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1236', 'Stabilo', 5000, TO_DATE('2021-05-01', 'yyyy-mm-dd'), 50);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1113', 'Lampu', 299000, TO_DATE('2021-03-17', 'yyyy-mm-dd'), 90);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1004', 'Keyboard', 620000, TO_DATE('2021-02-19', 'yyyy-mm-dd'), 27);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1005', 'Speaker', 115000, TO_DATE('2021-02-19', 'yyyy-mm-dd'), 14);
INSERT INTO inventory_items (inventory_id, inventory_item_title, inventory_item_price, inventory_item_timestamp, inventory_item_qty)
Values ('IVY1337', 'Watch', 50000, TO_DATE('2021-01-01', 'yyyy-mm-dd'), 0);

-- Carts_detail

INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1234', 'SC12345678', 'Available', TO_DATE('2021-03-01', 'yyyy-mm-dd'), 4);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1235', 'SC12345678', 'Available', TO_DATE('2021-04-30', 'yyyy-mm-dd'), 1);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1236', 'SC12345674', 'Available', TO_DATE('2021-05-01', 'yyyy-mm-dd'), 4);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1113', 'SC12345423', 'Available', TO_DATE('2021-03-17', 'yyyy-mm-dd'), 1);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1004', 'SC42315428', 'Available', TO_DATE('2021-02-19', 'yyyy-mm-dd'), 1);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1005', 'SC42315428', 'Available', TO_DATE('2021-02-19', 'yyyy-mm-dd'), 2);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1337', 'SC12345612', 'Not Available', TO_DATE('2021-05-24', 'yyyy-mm-dd'), 0);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1234', 'SC23856217', 'Available', TO_DATE('2021-03-01', 'yyyy-mm-dd'), 1);
INSERT INTO carts_detail (inventory_items_inventory_id, shopping_carts_cart_id, cart_detail_state, cart_detail_timestamp, cart_detail_qty)
Values ('IVY1005', 'SC23898721', 'Available', TO_DATE('2021-02-19', 'yyyy-mm-dd'), 1);

--Order Items

INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1234', 53243, 4, 'Sticky Notes', 'O501B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1235', 53244, 1, 'Flower Tumbler', 'O501B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1236', 54123, 4, 'Stabilo', 'O502B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1113', 51293, 1, 'Lampu', 'O503B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1004', 50924, 1, 'Keyboard', 'O504B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1005', 51826, 2, 'Speaker', 'O504B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1337', 51234, 1, 'Watch', 'O505B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1234', 55246, 1, 'Sticky Notes', 'O506B');
INSERT INTO order_items (inventory_items_inventory_id, order_item_code, order_item_qty, order_item_name, orders_order_id)
Values ('IVY1005', 55232, 1, 'Speaker', 'O507B');

--Orders Items Histories

INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (53243, 1, 4);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (53244, 2, 1);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (54123, 3, 4);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (51293, 4, 1);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (50924, 5, 1);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (51826, 6, 2);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (51234, 7, 1);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (55246, 8, 1);
INSERT INTO order_items_histories (order_items_order_item_code, history_sequence, history_qty)
Values (55232, 9, 1);

-- Addresses

INSERT INTO addresses (address_id, address_name, address_city, address_province, address_zip, users_user_id)
Values ('A1234', 'Jalan Melati 5 Blok H No.7', 'Jakarta Pusat', 'Jakarta', 16150, 'ECU12345678');
INSERT INTO addresses (address_id, address_name, address_city, address_province, address_zip, users_user_id)
Values ('A0002', 'Jalan Pegangsaan Timur no 45', 'Jakarta Barat', 'Jakarta', 16992, 'HNB25129988');
INSERT INTO addresses (address_id, address_name, address_city, address_province, address_zip, users_user_id)
Values ('A5432', 'Jalan Matoa 2 No. 2', 'Depok', 'Jawa Barat', 16435, 'JAE34567898');
INSERT INTO addresses (address_id, address_name, address_city, address_province, address_zip, users_user_id)
Values ('A2230', 'Jalan Sembiring 3 No.10', 'Bekasi', 'Jawa Barat', 18190, 'RFT23890122');

-- Shipping
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O501B', 54637, 'Reguler JNE', 5000, 'Complete', 'A1234');
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O502B', 12432, 'Reguler JNE', 5000, 'Complete', 'A1234');
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O503B', 13579, 'Sicepat', 9000, 'Complete', 'A0002');
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O504B', 12490, 'Sicepat', 9000, 'Complete', 'A0002');
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O506B', 23127, 'JNT', 5000, 'Complete', 'A2230');
INSERT INTO shippings (orders_order_id, shipping_number, ship_method, ship_charge, shipping_state, addresses_address_id)
Values ('O507B', 23138, 'JNT', 9000, 'Complete', 'A2230');

-- Ship Dates;
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-05-02', 'yyyy-mm-dd'), 'Delivered', 54637);
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-05-30', 'yyyy-mm-dd'), 'Delivered', 12432);
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-05-23', 'yyyy-mm-dd'), 'Delivered', 13579);
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-06-01', 'yyyy-mm-dd'), 'Delivered', 12490);
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-12-24', 'yyyy-mm-dd'), 'Delivered', 23127);
INSERT INTO ship_dates (ship_date, delivery_state, shippings_shipping_number)
Values ( TO_DATE('2021-12-29', 'yyyy-mm-dd'), 'Delivered', 23138);
