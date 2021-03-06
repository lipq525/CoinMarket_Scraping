Integrating a trading model to the platform.

There are two steps to follow: create a new strategy and running the trading platform.


A. Creating a new strategy.
	1 - Create a class that inherits Strategy in the file models.py.
		Example:

		class ToyStrategy(Strategy):
			pass

	2- Override the method calculate(). This method will be called each time a new trade is executed on the exchange. 
		Example:

		class ToyStrategy(Strategy):
			def calculate(self, _time, price, _type): 	
				pass

	3- Calculate receives the parameters '_time', 'price', and '_type' to calculate the trading logic. '_time' is a datetime object, price is a float with the last price traded, and '_type' will be a string containing 'bid' or 'ask'.
		Example:

		class ToyStrategy(Strategy):
			def calculate(self, _time, price, _type): 	
				if float(price) >= 3000:
					print(price)

	4- Send signals to the exchange calling the self.send_signal() method. This method receives a tuple that has this format (time, type, price). 'time' is a datetime object, type is a string containin 'BUY' or 'CLOSE' and price is a string containing the desired price of the order.
		Example:

		class ToyStrategy(Strategy):
			def calculate(self, _time, price, _type): 	
				if float(price) >= 3000:
					self.send_signal((_time, 'BUY', 3000.00))

	5- You can check the attribute 'self.accountState' to know if you are in or out of the market. This attribute can have two states: 'CLOSE' (out of the market) or 'BUY' (in the market).
		Example:

		class ToyStrategy(Strategy):
			def calculate(self, _time, price, _type): 	
				if (float(price) >= 3000) and (self.accountState == 'CLOSE'):
					self.send_signal((_time, 'BUY', 3000.00))
				elif float(price) <= 2000 and (self.accountState == 'BUY'):
					self.send_signal((_time, 'CLOSE', 2000.00))


B. Running the trading platform.
	1 - Change line 47 of the ./ccplatform/live_trader.py script so it will instantiate your strategy.
		Example:

		strategy = models.ToyStrategy()

	2 - Run the script on the command line. Optionally you can change the currency you want to trade with.
		Example:

		C://Codebase/data> python live_trader.py BTC-USD



Event-driven backtesting coming soon!

