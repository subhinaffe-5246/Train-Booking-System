<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RailExpress - Book Train Tickets</title>
    <style>
        /* CSS Styles */
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background-color: #003366;
            color: white;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        
        h1 {
            margin: 0;
            font-size: 2.5rem;
        }
        
        .tagline {
            font-style: italic;
            margin-top: 5px;
        }
        
        .booking-form {
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 30px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        
        button {
            background-color: #0066cc;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        
        button:hover {
            background-color: #0052a3;
        }
        
        .tickets-list {
            background-color: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .ticket {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 15px;
            margin-bottom: 15px;
            position: relative;
        }
        
        .ticket h3 {
            margin-top: 0;
            color: #0066cc;
        }
        
        .ticket-details {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        
        .ticket-info {
            flex: 1;
            min-width: 200px;
        }
        
        .delete-btn {
            background-color: #cc0000;
            position: absolute;
            top: 15px;
            right: 15px;
        }
        
        .delete-btn:hover {
            background-color: #a30000;
        }
        
        .no-tickets {
            text-align: center;
            color: #666;
            font-style: italic;
        }
        
        .summary {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid #eee;
        }
        
        @media (max-width: 768px) {
            .ticket-details {
                flex-direction: column;
            }
            
            .ticket-info {
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <h1>RailExpress</h1>
            <p class="tagline">Your journey begins here</p>
        </div>
    </header>
    
    <main class="container">
        <section class="booking-form">
            <h2>Book Your Ticket</h2>
            <form id="bookingForm">
                <div class="form-group">
                    <label for="from">From</label>
                    <select id="from" required>
                        <option value="">Select departure station</option>
                        <option value="New York">New York</option>
                        <option value="Chicago">Chicago</option>
                        <option value="Los Angeles">Los Angeles</option>
                        <option value="Washington DC">Washington DC</option>
                        <option value="Boston">Boston</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label for="to">To</label>
                    <select id="to" required>
                        <option value="">Select arrival station</option>
                        <option value="New York">New York</option>
                        <option value="Chicago">Chicago</option>
                        <option value="Los Angeles">Los Angeles</option>
                        <option value="Washington DC">Washington DC</option>
                        <option value="Boston">Boston</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label for="date">Travel Date</label>
                    <input type="date" id="date" required>
                </div>
                
                <div class="form-group">
                    <label for="passengers">Passengers</label>
                    <input type="number" id="passengers" min="1" max="10" value="1" required>
                </div>
                
                <div class="form-group">
                    <label for="class">Class</label>
                    <select id="class" required>
                        <option value="Economy">Economy</option>
                        <option value="Business">Business</option>
                        <option value="First">First Class</option>
                    </select>
                </div>
                
                <button type="submit">Book Ticket</button>
            </form>
        </section>
        
        <section class="tickets-list">
            <h2>Your Tickets</h2>
            <div id="ticketsContainer">
                <p class="no-tickets">No tickets booked yet</p>
            </div>
            <div class="summary">
                <strong>Total Tickets: <span id="totalTickets">0</span></strong>
                <strong>Total Passengers: <span id="totalPassengers">0</span></strong>
            </div>
        </section>
    </main>

    <script>
        // Ticket List ADT implementation
        class TicketList {
            constructor() {
                this.tickets = [];
                this.nextId = 1;
            }
            
            // Add a new ticket to the list
            addTicket(ticket) {
                ticket.id = this.nextId++;
                this.tickets.push(ticket);
                return ticket;
            }
            
            // Remove a ticket by ID
            removeTicket(id) {
                const index = this.tickets.findIndex(ticket => ticket.id === id);
                if (index !== -1) {
                    this.tickets.splice(index, 1);
                    return true;
                }
                return false;
            }
            
            // Get all tickets
            getAllTickets() {
                return [...this.tickets];
            }
            
            // Get total number of tickets
            getTotalTickets() {
                return this.tickets.length;
            }
            
            // Get total number of passengers across all tickets
            getTotalPassengers() {
                return this.tickets.reduce((total, ticket) => total + ticket.passengers, 0);
            }
        }

        // DOM elements
        const bookingForm = document.getElementById('bookingForm');
        const ticketsContainer = document.getElementById('ticketsContainer');
        const totalTicketsSpan = document.getElementById('totalTickets');
        const totalPassengersSpan = document.getElementById('totalPassengers');
        
        // Create ticket list instance
        const ticketList = new TicketList();
        
        // Format date for display
        function formatDate(dateString) {
            const options = { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' };
            return new Date(dateString).toLocaleDateString('en-US', options);
        }
        
        // Calculate price based on route and class
        function calculatePrice(from, to, travelClass, passengers) {
            // Base prices for routes
            const routePrices = {
                'New York-Chicago': 120,
                'New York-Washington DC': 80,
                'New York-Boston': 60,
                'Chicago-Los Angeles': 200,
                'Washington DC-Boston': 70,
                'default': 100
            };
            
            // Class multipliers
            const classMultipliers = {
                'Economy': 1,
                'Business': 1.5,
                'First': 2
            };
            
            // Determine route price
            const routeKey1 = `${from}-${to}`;
            const routeKey2 = `${to}-${from}`;
            let basePrice = routePrices[routeKey1] || routePrices[routeKey2] || routePrices['default'];
            
            // Apply class multiplier
            const pricePerPassenger = basePrice * classMultipliers[travelClass];
            
            // Calculate total
            return pricePerPassenger * passengers;
        }
        
        // Render all tickets
        function renderTickets() {
            const tickets = ticketList.getAllTickets();
            
            if (tickets.length === 0) {
                ticketsContainer.innerHTML = '<p class="no-tickets">No tickets booked yet</p>';
            } else {
                ticketsContainer.innerHTML = '';
                tickets.forEach(ticket => {
                    const ticketElement = document.createElement('div');
                    ticketElement.className = 'ticket';
                    ticketElement.innerHTML = `
                        <h3>Ticket #${ticket.id} - ${ticket.from} to ${ticket.to}</h3>
                        <div class="ticket-details">
                            <div class="ticket-info">
                                <p><strong>Date:</strong> ${formatDate(ticket.date)}</p>
                                <p><strong>Passengers:</strong> ${ticket.passengers}</p>
                            </div>
                            <div class="ticket-info">
                                <p><strong>Class:</strong> ${ticket.class}</p>
                                <p><strong>Price:</strong> $${ticket.price.toFixed(2)}</p>
                            </div>
                        </div>
                        <button class="delete-btn" data-id="${ticket.id}">Cancel Ticket</button>
                    `;
                    ticketsContainer.appendChild(ticketElement);
                });
                
                // Add event listeners to delete buttons
                document.querySelectorAll('.delete-btn').forEach(button => {
                    button.addEventListener('click', function() {
                        const id = parseInt(this.getAttribute('data-id'));
                        if (ticketList.removeTicket(id)) {
                            renderTickets();
                            updateSummary();
                        }
                    });
                });
            }
            
            updateSummary();
        }
        
        // Update summary information
        function updateSummary() {
            totalTicketsSpan.textContent = ticketList.getTotalTickets();
            totalPassengersSpan.textContent = ticketList.getTotalPassengers();
        }
        
        // Handle form submission
        bookingForm.addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Get form values
            const from = document.getElementById('from').value;
            const to = document.getElementById('to').value;
            const date = document.getElementById('date').value;
            const passengers = parseInt(document.getElementById('passengers').value);
            const travelClass = document.getElementById('class').value;
            
            // Validate from and to are different
            if (from === to) {
                alert('Departure and arrival stations must be different');
                return;
            }
            
            // Calculate price
            const price = calculatePrice(from, to, travelClass, passengers);
            
            // Create ticket object
            const ticket = {
                from,
                to,
                date,
                passengers,
                class: travelClass,
                price
            };
            
            // Add to list
            ticketList.addTicket(ticket);
            
            // Render tickets
            renderTickets();
            
            // Reset form
            bookingForm.reset();
        });
        
        // Initialize with a sample ticket if empty (for demo)
        if (ticketList.getTotalTickets() === 0) {
            const today = new Date();
            const tomorrow = new Date(today);
            tomorrow.setDate(today.getDate() + 1);
            const tomorrowStr = tomorrow.toISOString().split('T')[0];
            
            ticketList.addTicket({
                from: 'New York',
                to: 'Washington DC',
                date: tomorrowStr,
                passengers: 2,
                class: 'Business',
                price: 240
            });
            
            renderTickets();
        }
    </script>
</body>
</html>
