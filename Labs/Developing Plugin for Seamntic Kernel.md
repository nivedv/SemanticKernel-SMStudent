# Create Semantic Kernel Plugins

## Exercise Overview

In this exercise, you'll use Semantic Kernel to create an AI assistant that can search for and book flights for a user. You'll create custom plugin functions to help accomplish the task.

**Duration:** Approximately 15 minutes to complete

## Creating Plugin Functions

### Step 1: Create the Flight Booking Plugin

1. Edit the plugin file:

   **For Python:**

   ```bash
   code flight_booking_plugin.py
   ```

2. Add the search flights function:

   **Python:**

   ```python
   # Create a plugin function with kernel function attributes
   @kernel_function(description="Searches for available flights based on the destination and departure date in the format YYYY-MM-DD")
   def search_flights(self, destination, departure_date):
       # Filter flights based on destination
       matching_flights = [
           flight for flight in self.flights
           if flight.Destination.lower() == destination.lower() and flight.DepartureDate == departure_date
       ]
       return matching_flights
   ```

3. Add the book flight function:

   **Python:**

   ```python
   # Create a kernel function to book flights
   @kernel_function(description="Books a flight based on the flight ID provided")
   def book_flight(self, flight_id):
       # Add logic to book a flight
       flight = next((f for f in self.flights if f.Id == flight_id), None)
       if flight is None:
           return "Flight not found. Please provide a valid flight ID."
       if flight.IsBooked:
           return "You've already booked this flight."
       flight.IsBooked = True
       self.save_flights_to_file()
       return (
           f"Flight booked successfully! Airline: {flight.Airline}, "
           f"Destination: {flight.Destination}, Departure: {flight.DepartureDate}, "
           f"Price: ${flight.Price}."
       )
   ```

### Step 2: Register the Plugin

1. Edit the main program file:

   **For Python:**

   ```bash
   code plugins.py
   ```

2. Add the plugin to the kernel:

   **Python:**

   ```python
   # Add the plugin to the kernel
   kernel.add_plugin(FlightBookingPlugin(), "flight_booking_plugin")
   ```

3. Configure function choice behavior:

   **Python:**

   ```python
   # Configure function choice behavior
   settings = AzureChatPromptExecutionSettings(
       function_choice_behavior=FunctionChoiceBehavior.Auto(),
   )
   ```

### Step 3: Test the Complete Plugin

1. Save your changes (Ctrl+S)

2. Run the application:

   **Python:**

   ```bash
   python plugins.py
   ```

3. Expected output:

   ```
   User: Find me a flight to Tokyo on the 19
   Assistant: I found a flight to Tokyo on January 19th.
   - Airline: Air Japan
   - Price: $1200
   Would you like to book this flight?

   User: Yes
   Assistant: Congratulations! Your flight to Tokyo on January 19th with Air Japan has been successfully booked.
   The total price for the flight is $1200.
   ```

## Configuring Function Access Control

Now configure your kernel to only allow specific functions. The assistant should answer questions about flight availability but not allow booking.

1. Update the function choice behavior configuration:

   **Python:**

   ```python
   # Configure function choice behavior
   settings=AzureChatPromptExecutionSettings(
       function_choice_behavior=FunctionChoiceBehavior.Auto(filters={"included_functions": ["search_flights"]}),
   )
   ```

2. Save changes and run the code again

3. Expected restricted behavior:

   ```
   User: Find me a flight to Tokyo on the 19
   Assistant: I found a flight to Tokyo on the 19th of January 2025. The flight is with Air Japan and the price is $1200. Please let me know if you would like to book this flight.

   User: Yes
   Assistant: I'm sorry, but I am just a virtual assistant and I don't have the capability to book flights.
   ```

## Key Concepts

- **Kernel Function Decorators:** Help the AI understand how to call your functions
- **Function Choice Behavior:** Controls which functions are available to the AI
- **Plugin Registration:** Makes your custom functions available to the Semantic Kernel
- **Access Control:** Restricts which functions can be invoked by the AI assistant

## Conclusion

You have successfully created an AI assistant using Semantic Kernel that can search for and book flights using custom plugin functions. You've also learned how to control which functions are available to the AI assistant for different scenarios.
