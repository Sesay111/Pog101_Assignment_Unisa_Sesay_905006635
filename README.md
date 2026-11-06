 .Pog101_Assignment_Unisa_Sesay_905006635
Farm Wise SL:Crop Price and planting Advisor

1 Problem Description

Smallholder farmers in Sierra Leone often sell their crops without knowing what the crop is worth in nearby markets, and they often plant without clear, timely advice on the best planting window. Prices for rice, cassava, groundnut, maize and vegetables change by district and by week, but this information is usually passed by word of mouth. As a result, a farmer may accept a low price from a trader at the farm gate, or may plant too early or too late for the rains.
Who is affected: smallholder farmers (including many women farmers), rural households that depend on farming income, and local traders and extension officers who work without a shared, up-to-date source of information.
Why the problem exists: (a) there is no simple, central place where farmers can check current prices; (b) many rural areas have weak internet coverage and many farmers use basic phones rather than smartphones; (c) extension officers are few compared with the number of farmers; (d) price and season information is not recorded in a structured form.

2 Background

Agriculture is one of the most important sectors in Sierra Leone and supports the livelihoods of a large share of the population, with rice as the main staple food. The country has a clear rainy season (roughly May to October) and a dry season (roughly November to April), so the timing of planting strongly affects yields. Information is available from bodies such as the Ministry of Agriculture and Food Security and from market surveys, but it rarely reaches farmers quickly or in a form they can use. Mobile phones are widespread, and text-based services such as SMS and USSD work on basic handsets without internet data. This makes a phone-based advisory service realistic for the Sierra Leone context.

3 Community Impact

•Problem being solved: lack of timely market price information and planting guidance for smallholder farmers.

•Expected impact: farmers can compare prices before selling, plan their planting dates, and estimate income before they commit seed and labour. This can reduce post-harvest losses, improve bargaining power and increase household income.

•Beneficiaries: smallholder farmers, farmer cooperatives, rural households, extension officers and local markets.

Long-term sustainability: the logic is simple and modular, the data can be updated by trained community data agents and cooperatives, and the system can be maintained by local developers because it is open source (see Sections 1.7 and 1.8).

4 Proposed Solution

FarmWise SL is a menu-driven advisory system that a farmer reaches by a short phone menu. After a one-time registration (phone number and district), the farmer can: (1) check the latest price of a crop in their district, (2) get planting advice for a crop in the current month, (3) estimate expected income from a given farm size, and (4) find the market that currently pays the best price. The system checks every input, warns when price data is old, and alerts the farmer when a price drops sharply. This assignment covers the logical design only: algorithm, pseudocode, flowcharts, modules and expected outputs. No source code is written.

PROCEDURE 

 1. SET loginTries = 0
 2. SET loggedIn = FALSE
 3. WHILE loggedIn = FALSE AND loginTries < MAX_LOGIN_TRIES DO
 4.     INPUT agentId, agentPin
 5.     SET loggedIn = isAgentValid(agentId, agentPin)
 6.     IF loggedIn = FALSE THEN
 7.         SET loginTries = loginTries + 1
 8.         OUTPUT "Wrong ID or PIN."
 9.     END IF
10. END WHILE
11. IF loggedIn = FALSE THEN
12.     OUTPUT "Too many failed attempts. Account locked."
13.     STOP
14. END IF
15. SET market = readMarket()              // must be a known market
16. SET cropName = readCrop()              // must be a supported crop
17. SET newPrice = readPrice()             // validated number in range
18. SET oldPrice = getLatestPrice(cropName, market)
19. IF oldPrice IS FOUND THEN
20.     SET changePct = percentChange(oldPrice, newPrice)
21.     IF absoluteValue(changePct) > MAX_PRICE_JUMP THEN
22.         OUTPUT "Large change of " + changePct + " percent. Confirm? (Yes/No)"
23.         INPUT confirmed
24.         IF confirmed = "No" THEN
25.             OUTPUT "Update cancelled."
26.             STOP
27.         END IF
28.     END IF
29. END IF
30. CALL savePrice(cropName, market, newPrice, today())
31. CALL logUpdate(agentId, cropName, market, newPrice, now())
32. OUTPUT "Price saved: " + cropName + ", " + market + ", SLE " + newPrice
33. IF oldPrice IS FOUND AND changePct <= -PRICE_DROP_ALERT_PCT THEN
34.     CALL sendDropAlerts(cropName, market, newPrice, oldPrice)
35. END IF







