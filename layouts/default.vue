<template>
    <v-app>
        <v-navigation-drawer
            v-model="drawer"
            :mini-variant="miniVariant"
            :clipped="clipped"
            fixed
            app
        >
            <v-list>
                <v-list-item
                    v-for="(item, i) in items"
                    :key="i"
                    :to="item.to"
                    router
                    exact
                >
                    <v-list-item-action>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title v-text="item.title" />
                    </v-list-item-content>
                </v-list-item>

                <v-list-item @click.stop="miniVariant = !miniVariant">
                    <v-list-item-action>
                        <v-icon>
                            mdi-{{
                                `chevron-${miniVariant ? "right" : "left"}`
                            }}
                        </v-icon>
                    </v-list-item-action>
                    <v-list-item-content> Shrink </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <v-app-bar :clipped-left="clipped" fixed app>
            <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

            <v-toolbar-title v-text="title" />
            <v-spacer />
        </v-app-bar>

        <v-main>
            <v-container>
                <nuxt />
            </v-container>
        </v-main>

        <v-footer :absolute="!fixed" app>
            <span>&copy; {{ new Date().getFullYear() }}</span>
        </v-footer>
    </v-app>
</template>

<script>
export default {
    name: "DefaultLayout",

    data() {
        return {
            clipped: true,
            drawer: false,
            fixed: true,
            items: [
                {
                    icon: "mdi-apps",
                    title: "Calendar",
                    to: "/",
                },
                {
                    icon: "mdi-cloud-upload",
                    title: "Upload",
                    to: "/upload",
                },
            ],
            miniVariant: true,
            title: "Calendared",
        };
    },
};
</script>
