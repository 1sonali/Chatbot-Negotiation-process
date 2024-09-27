# Install OpenAI package (if not already installed)
!pip install openai
import openai

# Set your OpenAI API key
openai.api_key = '5902fe79-a604-401b-95f4-299305ce9ce0 us-east-1-awsus-east-1-aws'

# Set up initial pricing logic and negotiation details
class NegotiationBot:
    def __init__(self, product_name, base_price, min_price):
        self.product_name = product_name
        self.base_price = base_price
        self.current_price = base_price
        self.min_price = min_price
    
    def get_openai_response(self, prompt):
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "system", "content": "You are a negotiation bot."},
                      {"role": "user", "content": prompt}]
        )
        return response['choices'][0]['message']['content']
    
    def start_negotiation(self):
        print(f"Welcome to the negotiation for {self.product_name}!")
        print(f"The starting price is ${self.base_price}. Let's negotiate.")
        
        while True:
            user_input = input("Your offer: ")
            
            try:
                user_offer = float(user_input)
            except ValueError:
                print("Please enter a valid number.")
                continue
            
            # If user accepts the offer
            if user_offer >= self.current_price:
                print(f"Offer accepted at ${user_offer}!")
                break
            # If user offers less than the minimum price
            elif user_offer < self.min_price:
                print(f"Sorry, we can't go lower than ${self.min_price}.")
                continue
            else:
                # Counteroffer logic
                counter_offer = (user_offer + self.current_price) / 2
                self.current_price = counter_offer if counter_offer > self.min_price else self.min_price
                
                prompt = f"The customer offered ${user_offer}. The current price is ${self.current_price}. Respond politely."
                bot_response = self.get_openai_response(prompt)
                print(f"Bot: {bot_response}")
                
                print(f"My counteroffer is ${self.current_price}.")
                
                # Check if user accepts the counteroffer
                user_response = input("Do you accept this offer? (yes/no): ").strip().lower()
                if user_response == 'yes':
                    print(f"Deal closed at ${self.current_price}!")
                    break
                elif user_response == 'no':
                    print("Let's continue negotiating.")
                else:
                    print("Invalid input. Please type 'yes' or 'no'.")
# Initialize the negotiation bot with product name, base price, and minimum price
bot = NegotiationBot(product_name="Laptop", base_price=1000, min_price=700)

# Start the negotiation process
bot.start_negotiation()
!pip install textblob
from textblob import TextBlob
def analyze_sentiment(user_input):
    sentiment = TextBlob(user_input).sentiment
    return sentiment.polarity

# Integrate sentiment analysis into the negotiation flow
def start_negotiation(self):
    print(f"Welcome to the negotiation for {self.product_name}!")
    print(f"The starting price is ${self.base_price}. Let's negotiate.")
    
    while True:
        user_input = input("Your offer: ")
        
        try:
            user_offer = float(user_input)
        except ValueError:
            print("Please enter a valid number.")
            continue
        
        # Analyze sentiment of user input
        sentiment_score = analyze_sentiment(user_input)
        if sentiment_score > 0.5:
            print("You seem polite, I'll consider a better deal.")
            # Offer a better counteroffer if sentiment is positive
            discount = 0.05 * self.base_price
            self.current_price -= discount if self.current_price - discount >= self.min_price else 0
        
        # If user accepts the offer
        if user_offer >= self.current_price:
            print(f"Offer accepted at ${user_offer}!")
            break
        # If user offers less than the minimum price
        elif user_offer < self.min_price:
            print(f"Sorry, we can't go lower than ${self.min_price}.")
            continue
        else:
            # Counteroffer logic
            counter_offer = (user_offer + self.current_price) / 2
            self.current_price = counter_offer if counter_offer > self.min_price else self.min_price
            
            prompt = f"The customer offered ${user_offer}. The current price is ${self.current_price}. Respond politely."
            bot_response = self.get_openai_response(prompt)
            print(f"Bot: {bot_response}")
            
            print(f"My counteroffer is ${self.current_price}.")
            
            # Check if user accepts the counteroffer
            user_response = input("Do you accept this offer? (yes/no): ").strip().lower()
            if user_response == 'yes':
                print(f"Deal closed at ${self.current_price}!")
                break
            elif user_response == 'no':
                print("Let's continue negotiating.")
            else:
                print("Invalid input. Please type 'yes' or 'no'.")
