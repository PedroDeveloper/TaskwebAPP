<template>
    <div class="header-container">
        <header>
            <nav class="nav-menu"> 
                <div class="">
                    <a href="#" class="logo">dailyTasks</a>
                </div>

                <ul class="nav-list">
                    <li class="nav-item">
                        <a href="#" class="">Tarefas do Dia</a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="">Registrar Tarefa</a>
                    </li>
                   
                </ul>
               
                
            </nav>
        </header>
    </div>
</template>
<script>

export default {
    name: "Header"
}
</script>

<style lang="scss" scoped>
    * {
        font-family: 'Courier New', Courier, monospace;
        text-decoration: none;
        padding: 0;
        margin: 0;
        box-sizing: border-box;
        list-style: none;
    }

    .header-container {
        box-shadow: 5px 4px 10px rgba(0, 0, 0, 0.50);
        padding: 10px;
        margin-bottom: 30px;
        
    }

    .nav-menu {
        display: flex;
        justify-content:space-around;
        align-items: center;
        
    }

    .nav-list {
        display: flex;
        
    }

    .nav-item {
        padding: 0 10px;
    }

    .nav-item a {
        color: rgba(0, 0, 0, 1);
    }

    .nav-item:hover {
        opacity: 0.7;
    }



    .logo {
        letter-spacing:1px;
        color: rgba(0, 0, 0, 1);
    }


</style>