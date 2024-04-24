This is a very basic documentation of a very basic TMS. For evaluation purposes, some requirements
and use cases are lacking complete details. The candidate is expected to research, ask questions,
and exercise creative thinking to fill in the gaps and deliver a complete solution. We do not
expect high customizability, industry readiness, scalability, or anything other than a system that
works on a local environment and fulfills the most or all of the requirements. Requirements that
cannot be met within the timeframe agreed upon should be documented for discussing in the next
steps of the interviewing process; functional tests are more important than the feature itself if
you're unable to implement it.

Pro-tip: _do not overcomplicate._ Simplicity and clarity are appreciated. We recommend reading The
Zen of Python, by Tim Peters — or run `import this` on a Python 3 shell.


## Non-functional requirements

1. The system has to be divided into a backend and a frontend, following a three-tier architecture.
    1. The backend must be implemented with Python and the Flask and SQLAlchemy libraries.
    1. The frontend must be implemented with the React framework.
1. The application needs to run as Docker containers.
    1. The backend Docker image must inherit from the community-standard Python image.
    1. The frontend Docker image must inherit from the community-standard Node image.
    1. In the local environment, sattelite services such as database and cache servers must also
       run as Docker containers. This ensures that anyone in the team can easily have the same
       system without the need to configure infrastructure manually.
1. Processing of CSV files must happen asynchronously from HTTP requests.
1. Communication from the backend to a parallel worker process must happen through a message queue.
    1. The message queue must be provided by Redis.
1. Communication from the frontend to the backend must happen through RESTful APIs.
1. Communication from the backend to the frontend must happen through WebSockets.
1. All functionality must be covered by **functional** tests with Pytest.
    1. Test classes are named after the use cases, e.g. `class Test_user_imports_carriers`.
    1. Test methods are named after the scenarios, e.g. `def test_responds_400_if_missing_csv`.
1. The system must include documentation for deploying the application to a local or remote server.
    1. The documentation must include the steps to run the application locally.
    1. The documentation must include the steps to deploy the application to the cloud.


## Functional requirements

1. The application must implement the management of produce items.
    1. Produce items must have a name, unit (e.g. kg, box), and a category (e.g. fruit, vegetable).
1. The application must implement the management of carriers.
    1. Carriers must have a name, email, phone, company, and address.
    1. Carriers must have a list of produce items they can transport.
    1. The system must display the load history of a carrier.
1. The application must implement the management of loads.
    1. Loads must have a customer and a list of produce items to transport.
    1. Loads must have a status of "pending", "booked", "in transit", or "delivered".
1. The application must implement importing of carrier contacts from CSV files.
    1. The CSV file must have the columns: `name`, `email`, `phone`, `company`, `address`.
    1. The system must create a new carrier if the `email` is not already registered.
    1. The system must update an existing carrier if the `email` is already registered.
    1. The system must validate the `email` and `phone` columns.
    1. The system must validate the `company` column.


## Business rules

1. A carrier can only be assigned to a load if it is not busy.
    1. A carrier is busy if it is already assigned to a load that is not delivered.
1. A carrier can only be assigned to a load if it can transport all the produce items in the load.
1. A load cannot be updated or deleted once it's booked.
1. Certain fruits and vegetables produce gases that can speed up the decay or cause sprouting of
   other fruits and vegetables. Because of that, some produce items cannot be transported together
   in the same load.
    1. Apples and brocolli cannot be transported together.
    1. Bananas and lettuce cannot be transported together.
    1. Tomatoes and cucumbers cannot be transported together.
    1. Potatoes and onions cannot be transported together.
    1. Melons cannot be transported with any other produce item.


## Use cases

1. User imports carriers from CSV.
    - User opens the "Carrier Imports > Import from CSV" page.
    - User submits a CSV file with carrier contacts.
    - The system issues the processing of the CSV and responds with a HTTP 202 response.
    - The system signals the frontend when it finishes processing the CSV file.
1. User lists previous or ongoing carrier imports.
    - User opens the "Carrier Imports > List imports" page.
    - The system lists all previous and ongoing carrier imports.
        - For each import, the system shows the status, the number of new carriers, the number of
          updated carriers, and the number of carriers with errors.
1. User lists carriers.
    - User opens the "Carriers > List carriers" page.
    - The system lists all carriers.
        - For each carrier, the system shows the name, email, phone, and company.
        - For each carrier, the system shows whether the carrier is busy or not.
1. User checks details of a carrier.
    - User opens the "Carriers > List carriers" page.
    - User clicks on a carrier.
    - The system shows the carrier details.
        - The system shows the carrier's information.
        - The system shows the carrier's load history.
1. User lists customers.
    - User opens the "Customers > List customers" page.
    - The system lists all customers.
        - For each customer, the system shows the name and amount of pending and in transit loads.
1. User checks details of a customer.
    - User opens the "Customers > List customers" page.
    - User clicks on a customer.
    - The system shows the customer details.
        - The system shows the customer's information.
        - The system shows the customer's load history.
1. User lists loads.
    - User opens the "Loads > List loads" page.
    - The system lists all loads.
        - For each load, the system shows the customer, the status, and the carrier (if any).
1. User checks details of a load.
    - User opens the "Loads > List loads" page.
    - User clicks on a load.
    - The system shows the load details.
        - The system shows the produce items to transport.
        - The system shows the status.
        - The system shows the carrier (if any).
1. User assigns a carrier to a load.
    - User opens the "Loads > List loads" page.
    - User clicks on a load.
    - User clicks on the "Assign carrier" button.
    - User selects a carrier from the list.
        - Only carriers that can transport the produce items are shown.
        - Only carriers that are not busy are shown.
    - The system assigns the carrier to the load.


## Glossary

- **Carrier**: A freight company that transports produce items.
- **Produce item**: A product that is transported by carriers, e.g. tomato, banana, broccoli.
- **Customer**: A company that buys produce items, e.g. grocery stores, restaurants.
- **Load**: A set of produce items that are transported by a carrier, e.g. a truck fill.