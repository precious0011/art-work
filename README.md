
⸻

🔷 🧾 TESTING LOG TABLE

Test ID	Feature Tested	Test Data	Expected Result	Actual Result	Status	Fix Applied
T1	Register	Empty fields	Error message	Error shown	Pass	Validation added
T2	Register	Valid details	Account created	Account created	Pass	None
T3	Register	Same email twice	Error message	Duplicate allowed	Fail	Added email check
T4	Register	Same email again	Error message	Error shown	Pass	Fixed query
T5	Login	Wrong password	Error message	Error shown	Pass	None
T6	Login	Correct details	Login success	Failed	Fail	Fixed .strip() and .lower()
T7	Login	Correct details again	Login success	Success	Pass	Input cleaning works
T8	Dashboard	Load products	All products visible	Some missing	Fail	Synced with database
T9	Dashboard	Load products again	All products visible	All shown	Pass	Database updated
T10	Cart	Add item	Item added	Item added	Pass	None
T11	Cart	Checkout empty cart	Error	No error	Fail	Added empty check
T12	Cart	Checkout with items	Order saved	Order not saved	Fail	Added INSERT query
T13	Cart	Checkout again	Order saved	Success	Pass	Fixed database logic
T14	Orders	View history	Orders displayed	Empty	Fail	Fixed SELECT query
T15	Orders	View again	Orders displayed	Works	Pass	Query fixed
T16	Producer	View products	All products	Only some	Fail	Synced database
T17	Producer	View products again	All products	Works	Pass	Fixed
T18	Stock	Update stock	Stock changes	No change	Fail	Fixed UPDATE query
T19	Stock	Update again	Stock updates	Works	Pass	Query correct
T20	Orders (Producer)	View orders	Show all orders	Works	Pass	None

