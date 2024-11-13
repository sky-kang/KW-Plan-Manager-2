# KW-Plan-Manager-2

<!DOCTYPE html>
<html>

<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    
    <title>KW Plan Manager</title>
    <style>
        .container {
            width: 640px;
        }
    </style>
</head>

<body>

    <nav class="navbar bg-body-tertiary">
        <div class="container">
            <span class="navbar-brand mb-0 h1">
                <i class="bi bi-card-checklist"></i>
                KW-Plan-Manager
            </span>
        </div>
    </nav>
    <div class="container">
        <div class="d-flex align-items-center mb-2 mt-2">
            <input type="text" class="form-control" id="plan-input" placeholder="Enter a plan here">
            <button type="button" id="add-btn" class="btn btn-primary ms-1 text-nowrap">
                <i class="bi bi-plus"></i> Add
            </button>
        </div>

        <div class="d-flex">
            <div class="flex-grow-1 bg-light rounded-2 p-2 me-1 w-50">
                <h3>Plans</h3>
                <div id="plan-list"></div>
            </div>
            <div class="flex-grow-1 bg-light rounded-2 p-2 w-50">
                <h3>Done</h3>
                <div id="done-list"></div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

    <script>
        const Type = {
            Todo: 1,
            Done: 2
        };

        let plans = [];

        function savePlans() {
            localStorage.setItem("plans", JSON.stringify(plans));
        }


        function loadPlans() {
            let lastPlans = localStorage.getItem("plans");
            if (!lastPlans) return;

            plans = JSON.parse(lastPlans);
            plans.forEach(addToList);
        }

        // 3. Append the new Plan object to plans
        // 4. Create a new plan item and attach it to #plan-list.
        function addToList(plan) {
            let div = document.createElement("div");
            div.className = "plan bg-light p-1 rounded-2 ps-2 d-flex align-items-center";

            let span = document.createElement("span");
            span.classList.add("me-auto");
            span.textContent = plan.text;
            div.appendChild(span);

            if (plan.type === Type.Todo) {
                let buttonDone = document.createElement("button");
                buttonDone.classList.add("btn", "btn-sm", "btn-success", "me-1");
                buttonDone.innerHTML = '<i class="bi bi-check"></i>';
                buttonDone.addEventListener("click", () => {
                    plan.type = Type.Done;
                    div.remove();
                    addToList(plan);
                    savePlans();
                });
                div.appendChild(buttonDone);
            }

            let buttonRemove = document.createElement("button");
            buttonRemove.classList.add("btn", "btn-sm", "btn-danger");
            buttonRemove.innerHTML = '<i class="bi bi-x"></i>';
            buttonRemove.addEventListener("click", () => {
                div.remove();
                plans = plans.filter(p => p !== plan);
                savePlans();
            });
            div.appendChild(buttonRemove);

            let list = document.querySelector(plan.type === Type.Todo ? "#plan-list" : "#done-list");
            list.appendChild(div);
        }

        document.querySelector("#add-btn").addEventListener("click", () => {
            // 1. Read the text in #plan-input.
            let input = document.querySelector("#plan-input");
            
            let text = input.value;
            if (!text.length) return;

            // 2. Create a new Plan object.
            let plan = { text: text, type: Type.Todo };
            plans.push(plan);
            addToList(plan);
            savePlans();

            input.value = "";
        });

        window.addEventListener("load", loadPlans);
    </script>
</body>

</html>
