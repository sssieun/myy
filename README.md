<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Todo List</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background-color: #fffad9;
    font-size: 20px;
  }
  #todoList {
    max-width: 400px;
    margin: 0 auto;
  }
  #tasks {
    list-style: none;
    padding: 0;
  }
  #tasks li {
    display: flex;
    align-items: center;
    margin: 10px 0;
  }
  #tasks li input[type="checkbox"] {
    margin-right: 10px;
  }
  #buttons {
    
  }
</style>
</head>
<body>
  <div id="todoList">
    <h1 style="color: #6fa8d1; text-align:center;">Todo List</h1>
    <input type="text" id="taskInput" placeholder="오늘 할 일은?">
    <button id="addTaskButton" style="background:white; border:none; border-radius:50%;">+</button>
    <ul id="tasks"></ul>
    <div class="bottom">
      <div class="left" style="font-size:17px;">0개 남음</div>
      <span class="buttons" >
        <button class="showall" data-type="모두보기" style="background:#fcfcf0;  border-radius:20%; border-color:#b6e3fa;">모두보기</button>
        <button class="shownotyet" data-type="남은-일만-보기" style="background:#fcfcf0;  border-radius:20%; border-color:#b6e3fa;">남은 일만 보기</button>
        <button class="showdone" data-type="완료된-일-보기" style="background:#fcfcf0;  border-radius:20%; border-color:#b6e3fa;">완료된 일 보기</button>
      </span>
      <br></br>
      <button class="deletedone" style="background:#bed8e6; border:none; ">완료된 일 지우기</button>
    </div>
   <p class='info' style="font-size:14px;">목록이 보이지 않을 때에는 '모두 보기' 버튼을 눌러보세요!</p>
  </div>

  <script>
    const taskInput = document.getElementById("taskInput");
    const addTaskButton = document.getElementById("addTaskButton");
    const tasksList = document.getElementById("tasks");
    const showAllButton = document.querySelector(".showall");
    const showNotYetButton = document.querySelector(".shownotyet");
    const showDoneButton = document.querySelector(".showdone");
    const deleteDoneButton = document.querySelector(".deletedone");
    const leftCount = document.querySelector(".left");

    const tasks = [];

    addTaskButton.addEventListener("click", () => {
      const taskText = taskInput.value.trim();
      if (taskText !== "") {
        tasks.push({ text: taskText, done: false });
        renderTasks();
        taskInput.value = "";
      }
    });

    function renderTasks() {
      tasksList.innerHTML = "";
      let notDoneCount = 0;
      tasks.forEach((task, index) => {
        if (
          showAllButton.classList.contains("active") ||
          (showNotYetButton.classList.contains("active") && !task.done) ||
          (showDoneButton.classList.contains("active") && task.done)
        ) {
          const taskItem = document.createElement("li");
          taskItem.innerHTML = `
            <input type="checkbox" ${task.done ? "checked" : ""}>
            <span>${task.text}</span>
            <button>Delete</button>
          `;
          taskItem.querySelector("input").addEventListener("change", () => {
            tasks[index].done = !tasks[index].done;
            renderTasks();
          });
          taskItem.querySelector("button").addEventListener("click", () => {
            tasks.splice(index, 1);
            renderTasks();
          });
          tasksList.appendChild(taskItem);

          if (!task.done) {
            notDoneCount++;
          }
        }
      });

      leftCount.textContent = `${notDoneCount}개 남음`;
    }

    showAllButton.addEventListener("click", () => {
      showAllButton.classList.add("active");
      showNotYetButton.classList.remove("active");
      showDoneButton.classList.remove("active");
      renderTasks();
    });

    showNotYetButton.addEventListener("click", () => {
      showAllButton.classList.remove("active");
      showNotYetButton.classList.add("active");
      showDoneButton.classList.remove("active");
      renderTasks();
    });

    showDoneButton.addEventListener("click", () => {
      showAllButton.classList.remove("active");
      showNotYetButton.classList.remove("active");
      showDoneButton.classList.add("active");
      renderTasks();
    });

    deleteDoneButton.addEventListener("click", () => {
      for (let i = tasks.length - 1; i >= 0; i--) {
        if (tasks[i].done) {
          tasks.splice(i, 1);
        }
      }
      renderTasks();
    });

  </script>
</body>
</html>
