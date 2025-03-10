<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>奥特曼VS怪兽</title>
    <style>
        /* 战斗动画 */
        @keyframes attack {
            0% { transform: translateX(0); }
            50% { transform: translateX(30px); }
            100% { transform: translateX(0); }
        }
        .attack-animation {
            animation: attack 0.3s;
        }

        /* 血条样式 */
        .health-bar {
            width: 200px;
            height: 20px;
            background: #ddd;
            margin: 10px auto;
            border-radius: 10px;
            overflow: hidden;
        }
        .health-inner {
            height: 100%;
            background: #4CAF50;
            transition: width 0.5s;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 角色选择 -->
        <div id="role-select">
            <h2>选择奥特曼：</h2>
            <button onclick="selectUltraman('迪迦')">迪迦（光线技能）</button>
            <button onclick="selectUltraman('赛罗')">赛罗（剑术连击）</button>
        </div>

        <!-- 战斗界面 -->
        <div id="battle" style="display:none;">
            <div class="health-bar">
                <div id="ultraman-health" class="health-inner" style="width: 100%"></div>
            </div>
            <img id="ultraman-img" src="ultraman.png" class="character">
            
            <div class="health-bar">
                <div id="monster-health" class="health-inner" style="width: 100%"></div>
            </div>
            <img id="monster-img" src="monster.png" class="character">

            <!-- 技能按钮 -->
            <div id="skills"></div>
            <button onclick="attack()">普通攻击</button>
        </div>
    </div>

    <script>
        // 角色数据
        const roles = {
            迪迦: {
                hp: 300,
                attack: 30,
                skills: [
                    { name: "哉佩利敖光线", damage: 80, cooldown: 3 },
                    { name: "奥特屏障", effect: "防御提升50%", type: "buff" }
                ]
            },
            赛罗: {
                hp: 250,
                attack: 40,
                skills: [
                    { name: "赛罗双射线", damage: 100, cooldown: 4 },
                    { name: "头镖连击", damage: 60, cooldown: 2 }
                ]
            }
        };

        // 怪兽数据
        const monsters = {
            哥尔赞: { hp: 400, attack: 35, skill: "熔岩喷射" },
            贝利亚: { hp: 600, attack: 50, skill: "黑暗毁灭" }
        };

        let currentUltraman, currentMonster;

        // 选择奥特曼
        function selectUltraman(name) {
            currentUltraman = { ...roles[name], name };
            currentMonster = monsters["贝利亚"]; // 随机生成怪兽
            document.getElementById("role-select").style.display = "none";
            document.getElementById("battle").style.display = "block";
            updateHealth();
            loadSkills();
        }

        // 加载技能按钮
        function loadSkills() {
            const skillsDiv = document.getElementById("skills");
            skillsDiv.innerHTML = currentUltraman.skills.map(skill => `
                <button onclick="useSkill('${skill.name}')" 
                        ${skill.cooldown > 0 ? 'disabled' : ''}>
                    ${skill.name} (冷却: ${skill.cooldown}回合)
                </button>
            `).join('');
        }

        // 使用技能
        function useSkill(skillName) {
            const skill = currentUltraman.skills.find(s => s.name === skillName);
            if (skill.type === "buff") {
                // 处理增益效果
            } else {
                currentMonster.hp -= skill.damage;
            }
            updateHealth();
            triggerAnimation();
        }

        // 普通攻击
        function attack() {
            currentMonster.hp -= currentUltraman.attack;
            updateHealth();
            triggerAnimation();
            // 怪兽反击
            setTimeout(() => {
                currentUltraman.hp -= currentMonster.attack;
                updateHealth();
            }, 500);
        }

        // 更新血条
        function updateHealth() {
            document.getElementById("ultraman-health").style.width = 
                (currentUltraman.hp / roles[currentUltraman.name].hp * 100) + "%";
            document.getElementById("monster-health").style.width = 
                (currentMonster.hp / monsters[currentMonster.name].hp * 100) + "%";
        }

        // 触发攻击动画
        function triggerAnimation() {
            const img = document.getElementById("ultraman-img");
            img.classList.add("attack-animation");
            setTimeout(() => img.classList.remove("attack-animation"), 300);
        }
    </script>
</body>
</html>
