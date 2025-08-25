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

                    <input type="checkbox" v-model="habit.completed" />
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
    </div>
</template>

<script setup lang="ts">
interface Habit {
    id: number;
    name: string;
    completed: boolean;
}

const habits = useState<Habit[]>("habits", () => [
    { id: 1, name: "Drink Water", completed: false },
    { id: 2, name: "Exercise", completed: false },
    { id: 3, name: "Read", completed: false },
]);

function addHabit(name: string) {
    habits.value.push({ id: Date.now(), name, completed: false });
}

function removeHabit(id: number) {
    habits.value = habits.value.filter((habit) => habit.id !== id);
}
</script>
