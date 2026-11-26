# Down


"-1-
------------------------------------------------
class Tariff:
    def __init__(self, name, base_price, duration_weeks):
        self.name = name
        self.base_price = base_price
        self.duration_weeks = duration_weeks

    def calculate_final_price(self):
        return self.base_price

    def get_info(self):
        return f"Тариф: {self.name}, длительность: {self.duration_weeks} недель, цена: {self.calculate_final_price()} ₽"


class StandardTariff(Tariff):
    def __init__(self, name, base_price, duration_weeks, has_certificate=False):
        super().__init__(name, base_price, duration_weeks)
        self.has_certificate = has_certificate

    def get_info(self):
        info = super().get_info()
        if self.has_certificate:
            info += " + сертификат"
        return info


class PremiumTariff(Tariff):
    def __init__(self, name, base_price, duration_weeks, consultation_hours=0):
        super().__init__(name, base_price, duration_weeks)
        self.consultation_hours = consultation_hours

    def calculate_final_price(self):
        return super().calculate_final_price() + self.consultation_hours * 1500


class CorporateTariff(Tariff):
    def __init__(self, name, base_price, duration_weeks, company_name="", discount_percent=0):
        super().__init__(name, base_price, duration_weeks)
        self.company_name = company_name
        self.discount_percent = discount_percent

    def calculate_final_price(self):
        return super().calculate_final_price() * (1 - self.discount_percent / 100)

    def is_eligible_for_bulk(self, min_employees=10):
        return self.discount_percent >= min_employees




-2-
------------------------
from typing import List


class NotificationChannel:
    def send(self, message: str) -> bool:
        return True


class EmailChannel(NotificationChannel):
    def send(self, message: str) -> bool:
        print(f"Отправлено на email: {message}")
        return True


class TelegramChannel(NotificationChannel):
    def send(self, message: str) -> bool:
        print(f"Отправлено в Telegram: {message}")
        return True


class SMSChannel(NotificationChannel):
    def send(self, message: str) -> bool:
        print(f"Отправлено SMS: {message}")
        return True


class NotificationService:
    def __init__(self, channels: List[NotificationChannel]):
        self.channels = channels

    def broadcast(self, message: str):
        for channel in self.channels:
            channel.send(message)


email = EmailChannel()
tg = TelegramChannel()
sms = SMSChannel()

service = NotificationService([email, tg, sms])
service.broadcast("Ваш заказ №12345 готов к выдаче!")



-3-
----------------------------------------------------------------------
from typing import Dict


class CommissionCalculator:
    def __init__(self, base_rate: float = 0):
        self.base_rate = base_rate

    def calculate(self, price: float) -> float:
        return price * self.base_rate / 100


class ElectronicsCalculator(CommissionCalculator):
    def __init__(self):
        super().__init__(base_rate=10)

    def calculate(self, price: float) -> float:
        if price > 50000:
            return price * 8 / 100
        return super().calculate(price)


class ClothingCalculator(CommissionCalculator):
    def __init__(self):
        super().__init__(base_rate=12)

    def calculate(self, price: float) -> float:
        if price < 3000:
            return 200.0
        return super().calculate(price)


class GroceryCalculator(CommissionCalculator):
    def __init__(self):
        super().__init__(base_rate=7)

    def calculate(self, price: float) -> float:
        commission = super().calculate(price)
        return min(commission, 500.0)


class Marketplace:
    def __init__(self, calculators: Dict[str, CommissionCalculator]):
        self.calculators = calculators

    def get_total_commission(self, prices_by_category: Dict[str, float]) -> float:
        total = 0.0
        for category, price in prices_by_category.items():
            calculator = self.calculators[category]
            total += calculator.calculate(price)
        return total


marketplace = Marketplace({
    "electronics": ElectronicsCalculator(),
    "clothing": ClothingCalculator(),
    "grocery": GroceryCalculator(),
})

prices = {
    "electronics": 60000,
    "clothing": 2500,
    "grocery": 1200,
}

total = marketplace.get_total_commission(prices)
print("комиссия 8% = 4800")
print("комиссия 200 (фикс)")
print("комиссия 7% = 84")
print(f"ИТОГО: {total}")"
