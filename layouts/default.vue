<template>
    <nav>
        <v-app-bar :title="'Trendpop'" :elevation="2">
            <template v-slot:prepend>
                <v-app-bar-nav-icon></v-app-bar-nav-icon>
            </template>
        </v-app-bar>
    </nav>
    <v-main>
        <slot></slot>
    </v-main>
</template>