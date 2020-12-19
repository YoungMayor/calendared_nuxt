<template>
    <v-row justify="center" align="center">
        <v-col cols="12" sm="6" lg="5">
            <v-sheet v-if="$refs.calendar_one">
                <h1 class="text-center">
                    {{ $refs.calendar_one.title }}
                </h1>
            </v-sheet>

            <v-sheet class="d-flex justify-space-between">
                <v-btn class="ma-2" @click="$refs.calendar_one.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                    Previous
                </v-btn>

                <v-btn class="ma-2" @click="$refs.calendar_one.next()">
                    Next
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>

            <v-sheet height="600" class="d-flex align-center">
                <v-calendar
                    ref="calendar_one"
                    v-model="calendar_one_value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @change="getEvents"
                    @click:date="viewDay"
                ></v-calendar>
            </v-sheet>
        </v-col>

        <v-col cols="12" sm="6" lg="5">
            <v-sheet v-if="$refs.calendar_two">
                <h1 class="text-center">
                    {{ $refs.calendar_two.title }}
                </h1>
            </v-sheet>

            <v-sheet class="d-flex justify-space-between">
                <v-btn class="ma-2" @click="$refs.calendar_two.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                    Previous
                </v-btn>

                <v-btn class="ma-2" @click="$refs.calendar_two.next()">
                    Next
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>

            <v-sheet height="600" class="d-flex align-center">
                <v-calendar
                    ref="calendar_two"
                    v-model="calendar_two_value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @change="getEvents"
                    @click:date="viewDay"
                ></v-calendar>
            </v-sheet>
        </v-col>

        <v-bottom-sheet v-model="sheet" persistent scrollable>
            <v-card>
                <v-card-title> Date Opened here </v-card-title>

                <v-divider></v-divider>

                <v-card-text style="height: 300px">
                    <v-data-table
                        :headers="table_headers"
                        :items="table_events"
                        :items-per-page="5"
                        :loading="table_loading"
                        :mobile-breakpoint="120"
                        class="elevation-1"
                    ></v-data-table>
                </v-card-text>

                <v-divider></v-divider>

                <v-card-actions class="text-center">
                    <v-btn color="blue darken-1" text @click="sheet = false">
                        Close
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-bottom-sheet>
    </v-row>
</template>

<script>
export default {
    head() {
        return {
            title: "Calendar Home",
        };
    },

    name: "CalendarPage",

    data: () => ({
        sheet: false,
        type: "month",
        mode: "stack",
        weekday: [0, 1, 2, 3, 4, 5, 6],
        calendar_one_value: "",
        calendar_two_value: "",
        events: [],
        colors: [
            "blue",
            "indigo",
            "deep-purple",
            "cyan",
            "green",
            "orange",
            "grey darken-1",
        ],
        names: [
            "Meeting",
            "Holiday",
            "PTO",
            "Travel",
            "Event",
            "Birthday",
            "Conference",
            "Party",
        ],

        table_headers: [
            { text: "Event ID", align: "start", value: "id" },
            { text: "Auction Type", value: "type" },
            { text: "Initiator", value: "initiator" },
            { text: "Product Name", value: "name" },
            { text: "Weighted Average Price (BGN/MWh)", value: "wap" },
            { text: "Load (MW)", value: "load" },
            { text: "Number of Participants", value: "participants" },
        ],
        table_events: [],
        table_loading: false,
    }),

    methods: {
        getEvents({ start, end }) {
            const events = [];

            const min = new Date(`${start.date}T00:00:00`);
            const max = new Date(`${end.date}T23:59:59`);
            const days = (max.getTime() - min.getTime()) / 86400000;
            const eventCount = this.rnd(days, days + 20);

            for (let i = 0; i < eventCount; i++) {
                const firstTimestamp = this.rnd(min.getTime(), max.getTime());
                const first = new Date(
                    firstTimestamp - (firstTimestamp % 900000)
                );
                events.push({
                    name: "",
                    start: first,
                    color: "green",
                });
            }

            this.events = events;
        },

        getEventColor(event) {
            return event.color;
        },

        rnd(a, b) {
            return Math.floor((b - a + 1) * Math.random()) + a;
        },

        viewDay(payload) {
            console.log(payload);
            this.sheet = true;
            this.table_loading = true;
            this.table_events = [];

            setTimeout(() => {
                this.table_loading = false;
                this.table_events = [
                    {
                        id: "1335",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                    {
                        id: "1336",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                    {
                        id: "1337",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                ];
            }, 3500);
        },
    },
};
</script>
