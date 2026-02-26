# Module-8_Exercises
#Answer no 1

import mysql.connector
connection = mysql.connector.connect(
    host='localhost',
    port=3306,
    database='flight_game',
    user='root',
    password='Deva1965',
    autocommit=True
)

cursor = connection.cursor()

icao_input = input("Enter the ICAO code of an airport: ")
sql = f"SELECT name, municipality FROM airport WHERE ident = '{icao_input}'"

cursor.execute(sql)
result = cursor.fetchone()

if result:
    print(f"Airport Name: {result[0]}, Location (Town): {result[1]}")
else:
    print("Airport not found.")

cursor.close()

#Answer no 2

import mysql.connector

connection = mysql.connector.connect(
    host='localhost',
    port=3306,
    database='flight_game',
    user='root',
    password='Deva1965',
    autocommit=True
)

cursor = connection.cursor()


country_code = input("Enter the area code (e.g., FI): ")


sql = f"SELECT type, COUNT(*) FROM airport WHERE iso_country = '{country_code}' GROUP BY type"

cursor.execute(sql)
results = cursor.fetchall()

if results:
    print(f"Airports in {country_code}:")
    for row in results:

        print(f"- {row[1]} {row[0].replace('_', ' ')}s")
else:
    print("No data found for this area code.")

cursor.close()
connection.close()

#Answer no 3

from geopy.distance import geodesic
import mysql.connector


connection = mysql.connector.connect(
    host='localhost',
    port=3306,
    database='flight_game',
    user='root',
    password='Deva1965',
    autocommit=True
)
cursor = connection.cursor()

def get_airport_codes(icao):

    sql = "SELECT latitude_deg, longitude_deg FROM airport WHERE ident = %s"
    cursor.execute(sql, (icao,))
    return cursor.fetchone()

print("\nDistance Calculator")
icao1 = input("Enter the first airport ICAO code: ").upper()
icao2 = input("Enter the second airport ICAO code: ").upper()

codes1 = get_airport_codes(icao1)
codes2 = get_airport_codes(icao2)

if codes1 and codes2:
    distance = geodesic(codes1, codes2).kilometers
    print(f"The distance between {icao1} and {icao2} is {distance:.2f} km.")
else:
    print("Error: One or both ICAO codes not found in database.")

cursor.close()
connection.close()
