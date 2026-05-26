# Air Traffic Analysis - DAX Calculations

## Key Performance Indicators (KPIs)

### On-Time Performance
```dax
OnTimePerformance = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Flights),
        Flights[Status] = "On-Time"
    ),
    COUNTROWS(Flights),
    0
)

OnTimePercentage = [OnTimePerformance] * 100
```

### Average Delay
```dax
AverageDelay = 
AVERAGE(Flights[DelayMinutes])

AverageDelayRounded = 
ROUND([AverageDelay], 2)
```

### Cancellation Rate
```dax
CancellationRate = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Flights),
        Flights[Status] = "Cancelled"
    ),
    COUNTROWS(Flights),
    0
)

CancellationPercentage = [CancellationRate] * 100
```

### Total Flights
```dax
TotalFlights = 
COUNTROWS(Flights)

TotalFlightsOnTime = 
CALCULATE(
    COUNTROWS(Flights),
    Flights[Status] = "On-Time"
)

TotalFlightsDelayed = 
CALCULATE(
    COUNTROWS(Flights),
    Flights[Status] = "Delayed"
)

TotalFlightsCancelled = 
CALCULATE(
    COUNTROWS(Flights),
    Flights[Status] = "Cancelled"
)
```

### Total Passengers
```dax
TotalPassengers = 
SUM(Flights[Passengers])

TotalCapacity = 
SUM(Flights[Capacity])

LoadFactor = 
DIVIDE(
    [TotalPassengers],
    [TotalCapacity],
    0
)

LoadFactorPercentage = [LoadFactor] * 100
```

## Revenue Metrics

### Total Revenue
```dax
TotalRevenue = 
SUM(Revenue[TotalRevenue])

AverageRevenuePerFlight = 
DIVIDE(
    [TotalRevenue],
    [TotalFlights],
    0
)

RevenuePerPassenger = 
DIVIDE(
    [TotalRevenue],
    [TotalPassengers],
    0
)
```

### Ancillary Revenue
```dax
TotalAncillaryRevenue = 
SUM(Revenue[AncillaryRevenue])

AncillaryRevenuePercentage = 
DIVIDE(
    [TotalAncillaryRevenue],
    [TotalRevenue],
    0
) * 100
```

## Time-Based Calculations

### Month-over-Month Growth
```dax
CurrentMonthRevenue = 
CALCULATE(
    [TotalRevenue],
    MONTH(Flights[ScheduledDeparture]) = MONTH(TODAY())
)

MoMGrowth = 
DIVIDE(
    [CurrentMonthRevenue] - [PreviousMonthRevenue],
    [PreviousMonthRevenue],
    0
) * 100
```

### Year-over-Year Comparison
```dax
CurrentYearFlights = 
CALCULATE(
    [TotalFlights],
    YEAR(Flights[ScheduledDeparture]) = YEAR(TODAY())
)

YoYGrowth = 
DIVIDE(
    [CurrentYearFlights] - [PreviousYearFlights],
    [PreviousYearFlights],
    0
) * 100
```

## Route Analysis

### Flights by Route
```dax
FlightsByRoute = 
CALCULATE(
    [TotalFlights],
    Flights[Route]
)

PassengersByRoute = 
CALCULATE(
    [TotalPassengers],
    Flights[Route]
)

OnTimePerformanceByRoute = 
CALCULATE(
    [OnTimePercentage],
    Flights[Route]
)
```

## Airline Performance

### Airline On-Time Performance
```dax
AirlineOnTimePercentage = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Flights),
        Flights[Status] = "On-Time"
    ),
    COUNTROWS(Flights),
    0
) * 100
```

### Airline Load Factor
```dax
AirlineLoadFactor = 
CALCULATE(
    [LoadFactorPercentage],
    Airlines[AirlineCode]
)
```

## Airport Metrics

### Airport Traffic Volume
```dax
ArrivalFlights = 
CALCULATE(
    [TotalFlights],
    Flights[ArrivalAirport] = MAX(Airports[AirportCode])
)

DepartureFlights = 
CALCULATE(
    [TotalFlights],
    Flights[DepartureAirport] = MAX(Airports[AirportCode])
)

TotalAirportFlights = [ArrivalFlights] + [DepartureFlights]
```

### Airport Capacity Utilization
```dax
AirportCapacityUtilization = 
DIVIDE(
    CALCULATE(
        SUM(Flights[Passengers]),
        Flights[DepartureAirport] = MAX(Airports[AirportCode])
    ),
    CALCULATE(
        SUM(Flights[Capacity]),
        Flights[DepartureAirport] = MAX(Airports[AirportCode])
    ),
    0
) * 100
```

## Delay Analysis

### Delay Metrics
```dax
TotalDelayMinutes = 
SUM(Flights[DelayMinutes])

MaxDelayFlight = 
MAX(Flights[DelayMinutes])

MedianDelay = 
MEDIAN(Flights[DelayMinutes])
```

### Delay Reasons Distribution
```dax
WeatherDelayPercentage = 
DIVIDE(
    CALCULATE(
        [TotalFlights],
        Flights[DelayReason] = "Weather"
    ),
    [TotalFlights],
    0
) * 100
```

## Market Share

### Airline Market Share
```dax
AirlineMarketShare = 
DIVIDE(
    CALCULATE(
        [TotalPassengers],
        Airlines[AirlineCode]
    ),
    [TotalPassengers],
    0
) * 100
```

---

**Last Updated**: May 26, 2026  
**Version**: 1.0
