# buenasnoches
training


script db

CREATE DATABASE buenasnoches WITH OWNER =  me ENCODING = UTF8 ALLOW_CONNECTIONS = true CONNECTION LIMIT = -1 IS_TEMPLATE = true;


create user user_name with encrypted password 'mypassword';





BEGIN;
CREATE TABLE bitacora_reservaciones (
  bitacora_id INT GENERATED ALWAYS AS IDENTITY,
  hotel_name VARCHAR(255) NOT NULL,
  room_name VARCHAR(255) NOT NULL,
  guest_first_name VARCHAR(255) NOT NULL,
  guest_last_name VARCHAR(255) NOT NULL,
  guest_email VARCHAR(255) NOT NULL,
  start_date INT NOT NULL,
  end_date INT NOT NULL
);

CREATE TABLE hotel (
  hotel_id INT GENERATED ALWAYS AS IDENTITY,
  name VARCHAR NOT NULL,
  description VARCHAR(255) NOT NULL,
  stars INT,
  PRIMARY KEY (hotel_id)
);

CREATE TABLE reservation (
  reservation_id INT GENERATED ALWAYS AS IDENTITY,
  room_id INT NOT NULL,
  guest_id INT NOT NULL,
  start_date INT,
  end_date INT,
  check_in boolean,
  check_out boolean,
  PRIMARY KEY (reservation_id)
);

CREATE UNIQUE INDEX idx_room_id
ON reservation(room_id);

CREATE UNIQUE INDEX idx_guest_id
ON reservation(guest_id);

CREATE TABLE room (
  room_id INT GENERATED ALWAYS AS IDENTITY,
  hotel_id INT,
  description VARCHAR(255) NOT NULL,
  floor text NOT NULL,
  max_guests INT NOT NULL,
  type VARCHAR(255) NOT NULL,
  PRIMARY KEY (room_id),
  CONSTRAINT fk_hotel
    FOREIGN KEY (hotel_id)
      REFERENCES hotel(hotel_id),
  CONSTRAINT fk_reservation
    FOREIGN KEY (room_id)
      REFERENCES reservation(room_id)
);

CREATE TABLE guest_type (
  guest_type_id INT,
  name VARCHAR(255) NOT NULL,
  PRIMARY KEY (guest_type_id)
);

CREATE TABLE  guest (
  guest_id INT GENERATED ALWAYS AS IDENTITY,
  first_name VARCHAR(255),
  last_name VARCHAR(255),
  email VARCHAR(255),
  type INT NOT NULL,
  PRIMARY KEY (guest_id),
  CONSTRAINT fk_reservation_guest
    FOREIGN KEY (guest_id)
      REFERENCES reservation(guest_id)
);

ALTER TABLE guest
    ADD FOREIGN KEY (guest_id) REFERENCES guest_type (guest_type_id)
                DEFERRABLE INITIALLY DEFERRED;

ALTER TABLE guest_type 
    ADD FOREIGN KEY (guest_type_id) REFERENCES guest (guest_id)
      DEFERRABLE INITIALLY DEFERRED;


CREATE TABLE room_type (
  room_type_id INT,
  name VARCHAR(255) NOT NULL,
  PRIMARY KEY (room_type_id)
);

ALTER TABLE room
    ADD FOREIGN KEY (room_id) REFERENCES room_type (room_type_id)
                DEFERRABLE INITIALLY DEFERRED;

ALTER TABLE room_type 
    ADD FOREIGN KEY (room_type_id) REFERENCES room (room_id)
      DEFERRABLE INITIALLY DEFERRED;




COMMIT;




Alter table guest drop constraint    guest_guest_id_fkey;
Alter table guest_type drop constraint guest_type_guest_type_id_fkey;

Alter table room drop constraint    room_room_id_fkey;
Alter table room_type drop constraint room_type_room_type_id_fkey;




DROP TABLE IF EXISTS room;
DROP TABLE IF EXISTS guest;
DROP TABLE IF EXISTS guest_type;
DROP TABLE IF EXISTS room_type;

DROP TABLE IF EXISTS hotel;
DROP TABLE IF EXISTS reservation;

DROP TABLE IF EXISTS  bitacora_reservaciones ;


