<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Course Search</title>
  <style>
    body { font-family: sans-serif; padding: 20px; max-width: 1200px; margin: auto; }
    input, button, select { padding: 8px; margin: 5px 0; width: 100%; box-sizing: border-box; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 15px; }
    .card { border: 1px solid #ccc; border-radius: 10px; padding: 15px; display: flex; gap: 10px; align-items: center; background-color: #f9f9f9; }
    .card input { margin-bottom: 5px; }
    .avatar { width: 50px; height: 50px; border-radius: 50%; overflow: hidden; }
    .avatar img { width: 100%; height: 100%; object-fit: cover; }
    .button-delete { background-color: red; color: white; border: none; border-radius: 5px; cursor: pointer; padding: 5px 10px; }
    .button-add { background-color: green; color: white; border: none; border-radius: 5px; cursor: pointer; margin-top: 10px; padding: 10px; width: 100%; }
    .filter-row { display: flex; gap: 10px; margin-bottom: 15px; flex-wrap: wrap; }
  </style>
</head>
<body>

<h1>Course Search</h1>
<p style="color: green; font-weight: bold;">Done! I've added all the course details from the image into your app. Let me know if you'd like to group them by category, add filters, or anything else!</p>

<div class="filter-row">
  <input type="text" id="searchInput" placeholder="Search for a course or category (e.g., dance, music)">
  <select id="dayFilter">
    <option value="">Filter by Day</option>
    <option value="Monday">Monday</option>
    <option value="Tuesday">Tuesday</option>
    <option value="Wednesday">Wednesday</option>
    <option value="Thursday">Thursday</option>
    <option value="Friday">Friday</option>
    <option value="Saturday">Saturday</option>
    <option value="Sunday">Sunday</option>
  </select>
</div>

<div id="courseList" class="grid"></div>

<h2>Add New Course</h2>
<input type="text" id="name" placeholder="Course Name">
<input type="text" id="category" placeholder="Category (e.g., dance, music)">
<input type="text" id="day" placeholder="Day">
<input type="text" id="time" placeholder="Time">
<input type="text" id="room" placeholder="Room Number">
<select id="type">
  <option value="Group">Group</option>
  <option value="Individual">Individual</option>
</select>
<input type="number" id="students" placeholder="Number of Students (Group only)">
<input type="file" id="imageUpload" accept="image/*">
<button class="button-add" onclick="addCourse()">Add Course</button>

<script>
  const courses = [
    { name: "Bharatanatyam", category: "dance", day: "Saturday", time: "8:30 to 10:30am", room: "Dance Room A", type: "Group", students: 10, image: "https://via.placeholder.com/50" },
    { name: "Kathak", category: "dance", day: "Tuesday", time: "5:00 to 6:00pm", room: "Dance Room B", type: "Group", students: 8, image: "https://via.placeholder.com/50" },
    { name: "Kathak (Adv.)", category: "dance", day: "Wednesday", time: "5:00 to 6:00pm", room: "Dance Room B", type: "Individual", students: 1, image: "https://via.placeholder.com/50" },
    { name: "Odissi", category: "dance", day: "Thursday", time: "3:30 to 5:00pm", room: "Dance Room C", type: "Group", students: 6, image: "https://via.placeholder.com/50" },
    { name: "Zumba", category: "dance", day: "Monday", time: "5:00 to 6:00pm", room: "Fitness Hall", type: "Group", students: 15, image: "https://via.placeholder.com/50" },
    { name: "Zumba", category: "dance", day: "Friday", time: "5:00 to 6:00pm", room: "Fitness Hall", type: "Group", students: 15, image: "https://via.placeholder.com/50" }
  ];

  function renderCourses() {
    const search = document.getElementById('searchInput').value.toLowerCase();
    const dayFilter = document.getElementById('dayFilter').value.toLowerCase();
    const list = document.getElementById('courseList');
    list.innerHTML = '';

    courses.filter(course => {
      return (
        (course.name.toLowerCase().includes(search) || course.category.toLowerCase().includes(search)) &&
        (dayFilter === '' || course.day.toLowerCase().includes(dayFilter))
      );
    })
    .forEach((course, idx) => {
      const card = document.createElement('div');
      card.className = 'card';

      card.innerHTML = `
        <div class="avatar"><img src="${course.image}" alt="Teacher"></div>
        <div style="flex: 1">
          <input value="${course.name}" onchange="updateCourse(${idx}, 'name', this.value)">
          <input value="${course.category}" onchange="updateCourse(${idx}, 'category', this.value)">
          <input value="${course.day}" onchange="updateCourse(${idx}, 'day', this.value)">
          <input value="${course.time}" onchange="updateCourse(${idx}, 'time', this.value)">
          <input value="${course.room}" onchange="updateCourse(${idx}, 'room', this.value)">
          <input value="${course.type}" onchange="updateCourse(${idx}, 'type', this.value)">
          <input value="${course.students}" onchange="updateCourse(${idx}, 'students', this.value)">
          <input value="${course.image}" onchange="updateCourse(${idx}, 'image', this.value)">
          <button class="button-delete" onclick="deleteCourse(${idx})">Delete</button>
        </div>
      `;
      list.appendChild(card);
    });
  }

  function addCourse() {
    const name = document.getElementById('name').value;
    const category = document.getElementById('category').value;
    const day = document.getElementById('day').value;
    const time = document.getElementById('time').value;
    const room = document.getElementById('room').value;
    const type = document.getElementById('type').value;
    const students = parseInt(document.getElementById('students').value) || 0;
    const imageFile = document.getElementById('imageUpload').files[0];

    if (name && category && day && time && room && type && imageFile) {
      const reader = new FileReader();
      reader.onload = function(e) {
        courses.push({ name, category, day, time, room, type, students, image: e.target.result });
        document.getElementById('name').value = '';
        document.getElementById('category').value = '';
        document.getElementById('day').value = '';
        document.getElementById('time').value = '';
        document.getElementById('room').value = '';
        document.getElementById('type').value = 'Group';
        document.getElementById('students').value = '';
        document.getElementById('imageUpload').value = '';
        renderCourses();
      };
      reader.readAsDataURL(imageFile);
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
  document.getElementById('dayFilter').addEventListener('change', renderCourses);
  window.onload = renderCourses;
</script>

</body>
</html>

