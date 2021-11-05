## Information
- target: handle all online hotel activities easily and safely
- keep track of all the available rooms
- book rooms
- generate bills
  
## Requirements
### My opinion
1. book, several types of room
2. un-book
3. search available rooms
4. 

### Standard
1. The system should support the booking of different room types like standard, deluxe, family suite, etc. (book all types)
2. Guests should be able to search the room inventory and book any available room. (search and book)
3. The system should be able to retrieve information, such as who booked a particular room, or what rooms were booked by a specific customer. (retrieve)
4. The system should allow customers to cancel their booking - and provide them with a full refund if the cancelation occurs before 24 hours of the check-in date. (cancel booking)
5. The system should be able to send notifications whenever the booking is nearing the check-in or check-out date. (send notifacations)
6. Pay, pay type: credit card, check or cash


### Use case diagram
#### Main Actors in our system
1. Guest
2. Receptionlist
3. System
4. Manager
5. Housekeeper
6. Server

#### Top use cases of the Hotel Management System
1. Add/Remove/Edit room
2. Search room
3. Register or cancel an account
4. Book room
5. Check-in
6. Check-out
7. Add room charge
8. Update housekeeping log


```python

# EXPLAIN: Enums, data types, and constants: Here are the required enums, data types, and constants
from abc import ABC, abstractmethod


class RoomStyle(Enum):
    STANDARD, DELUXE, FAMILY_SUITE, BUSINESS_SUITE = 1, 2, 3, 4


class RoomStatus(Enum):
    AVAILABLE, RESERVED, OCCUPIED, NOT_AVAILABLE, BEING_SERVICED, OTHER = 1, 2, 3, 4, 5, 6


class BookingStatus(Enum):
    REQUESTED, PENDING, CONFIRMED, CHECKED_IN, CHECKED_OUT, CANCELLED, ABANDONED = 1, 2, 3, 4, 5, 6, 7


class AccountStatus(Enum):
    ACTIVE, CLOSED, CANCELED, BLACKLISTED, BLOCKED = 1, 2, 3, 4, 5


class AccountType(Enum):
    MEMBER, GUEST, MANAGER, RECEPTIONIST = 1, 2, 3, 4


class PaymentStatus(Enum):
    UNPAID, PENDING, COMPLETED, FILLED, DECLINED, CANCELLED, ABANDONED, SETTLING, SETTLED, REFUNDED = 1, 2, 3, 4, 5, 6, 7, 8, 9, 10


class Address:
    def __init__(self, street, city, state, zip_code, country):
        self.__street_address = street
        self.__city = city
        self.__state = state
        self.__zip_code = zip_code
        self.__country = country


# EXPLAIN: Account, Person, Guest, Receptionist, and Server: These classes represent the different people that interact with our system
# For simplicity, we are not defining getter and setter functions. The reader can
# assume that all class attributes are private and accessed through their respective
# public getter methods and modified only through their public methods function.

class Account:
    def __init__(self, id, password, status=AccountStatus.Active):
        self.__id = id
        self.__password = password
        self.__status = status

    def reset_password(self):
        None


# from abc import ABC, abstractmethod
class Person(ABC):
    def __init__(self, name, address, email, phone, account):
        self.__name = name
        self.__address = address
        self.__email = email
        self.__phone = phone
        self.__account = account


class Guest(Person):
    def __init__(self):
        self.__total_rooms_checked_in = 0

    def get_bookings(self):
        None


class Receptionist(Person):
    def search_member(self, name):
        None

    def create_booking(self):
        None


class Server(Person):
    def add_room_charge(self, room, room_charge):
        None


# EXPLAIN: Hotel and HotelLocation: These classes represent the top-level classes of the system

class HotelLocation:
    def __init__(self, name, address):
        self.__name = name
        self.__location = address

    def get_rooms(self):
        None


class Hotel:
    def __init__(self, name):
        self.__name = name
        self.__locations = []

    def add_location(self, location):
        None


# EXPLAIN: Room, RoomKey, and RoomHouseKeeping: To encapsulate a room, room key, and housekeeping

class Search(ABC):
    def search(self, style, start_date, duration):
        None


class Room(Search):
    def __init__(self, room_number, room_style, status, price, is_smoking):
        self.__room_number = room_number
        self.__style = room_style
        self.__status = status
        self.__booking_price = price
        self.__is_smoking = is_smoking

        self.__keys = []
        self.__house_keeping_log = []

    def is_room_available(self):
        None

    def check_in(self):
        None

    def check_out(self):
        None

    def search(self, style, start_date, duration):
        # return all rooms with the given style and availability
        None


class RoomKey:
    def __init__(self, key_id, barcode, is_active, is_master):
        self.__key_id = key_id
        self.__barcode = barcode
        self.__issued_at = datetime.date.today()
        self.__active = is_active
        self.__is_master = is_master

    def assign_room(self, room):
        None

    def is_active(self):
        None


class RoomHouseKeeping:
    def __init__(self, description, duration, house_keeper):
        self.__description = description
        self.__start_datetime = datetime.date.today()
        self.__duration = duration
        self.__house_keeper = house_keeper

    def add_house_keeping(self, room):
        None


# EXPLAIN: RoomBooking and RoomCharge: To encapsulate a booking and different charges against a booking

class RoomBooking:
    def __init__(self, reservation_number, start_date, duration_in_days, booking_status):
        self.__reservation_number = reservation_number
        self.__start_date = start_date
        self.__duration_in_days = duration_in_days
        self.__status = booking_status
        self.__checkin = None
        self.__checkout = None

        self.__guest_id = 0
        self.__room = None
        self.__invoice = None
        self.__notifications = []

    def fetch_details(self, reservation_number):
        None


# from abc import ABC, abstractmethod
class RoomCharge(ABC):
    def __init__(self):
        self.__issue_at = datetime.date.today()

    def add_invoice_item(self, invoice):
        None


class Amenity(RoomCharge):
    def __init__(self, name, description):
        self.__name = name
        self.__description = description


class RoomService(RoomCharge):
    def __init__(self, is_chargeable, request_time):
        self.__is_chargeable = is_chargeable
        self.__request_time = request_time


class KitchenService(RoomCharge):
    def __init__(self, description):
        self.__description = description


```











