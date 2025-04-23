<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Course Search</title>
  <style>
    body { font-family: sans-serif; padding: 20px; max-width: 800px; margin: auto; }
    input, button { padding: 8px; margin: 5px 0; width: 100%; box-sizing: border-box; }
    .card { border: 1px solid #ccc; border-radius: 10px; padding: 15px; margin-bottom: 10px; display: flex; gap: 10px; align-items: center; }
    .card input { margin-bottom: 5px; }
    .avatar { width: 50px; height: 50px; border-radius: 50%; overflow: hidden; }
    .avatar img { width: 100%; height: 100%; object-fit: cover; }
    .button-delete { background-color: red; color: white; border: none; border-radius: 5px; cursor: pointer; }
    .button-add { background-color: green; color: white; border: none; border-radius: 5px; cursor: pointer; margin-top: 10px; }
  </style>
</head>
<body>

<h1>Course Search</h1>
<input type="text" id="searchInput" placeholder="Search for a course">

<div id="courseList"></div>

<h2>Add New Course</h2>
<input type="text" id="name" placeholder="Course Name">
<input type="text" id="day" placeholder="Day">
<input type="text" id="time" placeholder="Time">
<input type="text" id="room" placeholder="Room">
<input type="text" id="image" placeholder="Teacher Image URL">
<button class="button-add" onclick="addCourse()">Add Course</button>

<script>
  const courses = [
    { name: "Bharatanatyam", day: "Saturday", time: "8:30 to 10:30am", room: "Dance Room A", image: "https://via.placeholder.com/50" },
    { name: "Kathak", day: "Tues & Wed (Adv.)", time: "5:00 to 6:00pm", room: "Dance Room B", image: "https://via.placeholder.com/50" },
    { name: "Odissi", day: "Thursday", time: "3:30 to 5:00pm", room: "Dance Room C", image: "https://via.placeholder.com/50" },
    { name: "Zumba", day: "Mon & Fri", time: "5:00 to 6:00pm", room: "Fitness Hall", image: "https://via.placeholder.com/50" }
  ];

  function renderCourses() {
    const search = document.getElementById('searchInput').value.toLowerCase();
    const list = document.getElementById('courseList');
    list.innerHTML = '';

    courses.filter(course => course.name.toLowerCase().includes(search))
      .forEach((course, idx) => {
        const card = document.createElement('div');
        card.className = 'card';

        card.innerHTML = `
          <div class="avatar"><img src="${course.image}" alt="Teacher"></div>
          <div style="flex: 1">
            <input value="${course.name}" onchange="updateCourse(${idx}, 'name', this.value)">
            <input value="${course.day}" onchange="updateCourse(${idx}, 'day', this.value)">
            <input value="${course.time}" onchange="updateCourse(${idx}, 'time', this.value)">
            <input value="${course.room}" onchange="updateCourse(${idx}, 'room', this.value)">
            <input value="${course.image}" onchange="updateCourse(${idx}, 'image', this.value)">
            <button class="button-delete" onclick="deleteCourse(${idx})">Delete</button>
          </div>
        `;
        list.appendChild(card);
      });
  }

  function addCourse() {
    const name = document.getElementById('name').value;
    const day = document.getElementById('day').value;
    const time = document.getElementById('time').value;
    const room = document.getElementById('room').value;
    const image = document.getElementById('image').value;
    if (name && day && time && room && image) {
      courses.push({ name, day, time, room, image });
      document.getElementById('name').value = '';
      document.getElementById('day').value = '';
      document.getElementById('time').value = '';
      document.getElementById('room').value = '';
      document.getElementById('image').value = '';
      renderCourses();
    }
  }

  function deleteCourse(index) {
    courses.splice(index, 1);
    renderCourses();
  }

  function updateCourse(index, key, value) {
    courses[index][key] = value;
  }

  document.getElementById('searchInput').addEventListener('input', renderCourses);
  window.onload = renderCourses;
</script>

</body>
</html>
