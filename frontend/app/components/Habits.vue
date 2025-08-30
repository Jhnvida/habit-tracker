<template>
    <div class="w-full py-12 px-16">
        <div class="grid grid-cols-3 gap-2">
            <div v-for="habit in habits" class="flex flex-row items-center justify-between border py-2 px-4"
                :class="habit.editing ? 'border-blue-400' : 'border-gray-300'">
                <p v-if="!habit.editing">{{ habit.name }}</p>

                <input v-if="habit.editing" type="text" v-model="habit.name"
                    class="w-full border-none outline-none focus:outline-none focus:ring-0 bg-transparent p-0" />

                <div class="flex gap-2">
                    <Icon v-if="habit.editing" class="cursor-pointer" name="ic:baseline-save"
                        @click="editHabit(habit.id, false)" />

                    <Icon v-else class="cursor-pointer" name="ic:baseline-edit" @click="editHabit(habit.id, true)" />

                    <Icon class="cursor-pointer" name="ic:baseline-delete" @click="removeHabit(habit.id)" />

                    <Icon v-if="habit.completed" class="cursor-pointer" name="ic:baseline-check-box"
                        @click="updateHabit(habit.id, false)" />

                    <Icon v-else class="cursor-pointer" name="ic:baseline-check-box-outline-blank"
                        @click="updateHabit(habit.id, true)" />
                </div>
            </div>

            <button class="bg-black text-white py-3 px-5 text-sm font-medium cursor-pointer" type="button"
                @click="addHabit('New Habit')">
                New Habits
            </button>
        </div>
    </div>
</template>

<script setup lang="ts">
onMounted(() => getHabits());

interface Habit {
    id: number;
    name: string;
    completed: boolean;
    editing: boolean;
}

const habits = useState<Habit[]>("habits", () => []);

function updateStorage() {
    localStorage.setItem("habits", JSON.stringify(habits.value));
}

function getHabits() {
    const storedHabits = localStorage.getItem("habits");

    if (storedHabits) {
        habits.value = JSON.parse(storedHabits);
    }
}

function addHabit(name: string) {
    habits.value.push({
        id: Date.now(),
        name,
        completed: false,
        editing: false,
    });

    updateStorage();
}

function removeHabit(id: number) {
    habits.value = habits.value.filter((habit) => habit.id !== id);
    updateStorage();
}

function updateHabit(id: number, completed: boolean) {
    const habit = habits.value.find((habit) => habit.id == id);
    if (habit) habit.completed = completed;

    updateStorage();
}

function editHabit(id: number, editing: boolean) {
    const habit = habits.value.find((habit) => habit.id == id);
    if (habit) habit.editing = editing;

    updateStorage();
}
</script>