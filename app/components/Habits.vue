<template>
    <div class="w-full py-12 px-16">
        <div class="grid grid-cols-3 gap-2">
            <div
                v-for="habit in habits"
                class="flex flex-row items-center justify-between border border-gray-300 py-2 px-4"
            >
                <p>{{ habit.name }}</p>

                <div class="flex gap-2">
                    <Icon
                        class="cursor-pointer"
                        name="ic:baseline-delete"
                        @click="removeHabit(habit.id)"
                    />

                    <Icon
                        v-if="habit.completed"
                        name="ic:baseline-check-box"
                        @click="updateHabit(habit.id, !habit.completed)"
                    />

                    <Icon
                        v-else
                        name="ic:baseline-check-box-outline-blank"
                        @click="updateHabit(habit.id, !habit.completed)"
                    />
                </div>
            </div>

            <button
                class="bg-black text-white py-3 px-5 text-sm font-medium cursor-pointer"
                type="button"
                @click="addHabit('New Habit')"
            >
                New Habits
            </button>
        </div>

        <pre>{{ habits }}</pre>
    </div>
</template>

<script setup lang="ts">
onMounted(() => getHabits());

interface Habit {
    id: number;
    name: string;
    completed: boolean;
}

const habits = useState<Habit[]>("habits", () => []);

function getHabits() {
    const storedHabits = localStorage.getItem("habits");

    if (storedHabits) {
        habits.value = JSON.parse(storedHabits);
    }
}

function addHabit(name: string) {
    habits.value.push({ id: Date.now(), name, completed: false });
    localStorage.setItem("habits", JSON.stringify(habits.value));
}

function removeHabit(id: number) {
    habits.value = habits.value.filter((habit) => habit.id !== id);
    localStorage.setItem("habits", JSON.stringify(habits.value));
}

function updateHabit(id: number, completed: boolean) {
    const habit = habits.value.find((habit) => habit.id == id);
    if (habit) habit.completed = completed;

    localStorage.setItem("habits", JSON.stringify(habits.value));
}
</script>
