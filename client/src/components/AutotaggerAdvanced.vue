<template>
<div class='text-left q-pt-xs' style='max-width: 450px; margin:auto;'>
    <q-list>
        <!-- Overwrite tags -->
        <AdvancedSettingsToggle 
            label='Overwrite tags'
            tooltip='When disabling toggle, you can select which tags to overwrite alternatively'
            v-model='$1t.config.value.overwrite'
        ></AdvancedSettingsToggle>

        <!-- Overwrite select -->
        <div v-if='!$1t.config.value.overwrite' class='q-pb-md'>
            <q-select
                dark
                standout='text-grey-4 bg-dark'
                class='row select q-ma-auto q-mt-xl'
                :model-value='SUPPORTED_TAGS.filter(t => $1t.config.value.overwriteTags.includes(t.tag))'
                @update:model-value='(v) => $1t.config.value.overwriteTags = v.map((v: any)=> v.tag)'
                :options='SUPPORTED_TAGS'
                use-chips
                multiple
                label='Select which tags to overwrite'
            >
                <template v-slot:selected-item='scope'>
                    <q-chip 
                        color='primary' 
                        :label='scope.opt.label' 
                        class='text-black'
                        removable
                        @remove='scope.removeAtIndex(scope.index)'
                    ></q-chip>
                </template>
            </q-select>
        </div>

        <AdvancedSettingsToggle 
            label='ID3v2.4'
            tooltip='Use ID3v2.4 instead of IDv2.3 for MP3/AIFF files'
            v-model='$1t.config.value.id3v24'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Short title'
            tooltip='Write title without version, currently supported for Beatport & Traxsource only'
            v-model='$1t.config.value.shortTitle'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Save album art to file'
            tooltip='Writes a cover.jpg into the folder'
            v-model='$1t.config.value.albumArtFile'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Merge/append genre & styles'
            tooltip='Merge the fetched genre & styles with existing ones, instead of replacing'
            v-model='$1t.config.value.mergeGenres'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Camelot key notation'
            tooltip='Write Camelot key 8A, 8B ... instead of musical key Am, C ...'
            v-model='$1t.config.value.camelot'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Track or Release ID tag as input to get exact match'
            tooltip='Valid tags are: DISCOGS_RELEASE_ID, BEATPORT_TRACK_ID'
            v-model='$1t.config.value.matchById'
        ></AdvancedSettingsToggle>

        <div class='row'>
            <img width='16' class='q-ml-md' src='../assets/shazam_icon.svg' svg-inline />
            <AdvancedSettingsToggle 
                label='Identify tracks with Shazam'
                tooltip='Match tracks without any tags using Shazam'
                v-model='$1t.config.value.enableShazam'
                style='width: calc(100% - 32px)'
            ></AdvancedSettingsToggle>
        </div>
        <AdvancedSettingsToggle 
            label='Force Shazam'
            tooltip='Indentify tracks with Shazam, even if tags are present'
            v-model='$1t.config.value.forceShazam'
            v-if='$1t.config.value.enableShazam'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Skip already tagged tracks'
            tooltip='Skip tracks with the 1T_TAGGEDDATE tag (One Tagger Tag)'
            v-model='$1t.config.value.skipTagged'
        ></AdvancedSettingsToggle>
        
        <AdvancedSettingsToggle 
            label='Include subfolders'
            tooltip='Tag all subfolders as well'
            v-model='$1t.config.value.includeSubfolders'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Only write year'
            tooltip='Write just the year instead of full date'
            v-model='$1t.config.value.onlyYear'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Tag same track on multiple platforms'
            tooltip='Tag every track on every platform to get all the tags'
            v-model='$1t.config.value.multiplatform'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Remove all covers if overwriting'
            tooltip='Removes all existing covers if overwriting album art'
            v-model='$1t.config.value.removeAllCovers'
        ></AdvancedSettingsToggle>

        <!-- Parse meta from filename -->
        <AdvancedSettingsToggle 
            label='Parse metadata from filename'
            tooltip='Valid variables are: %track%, %artist%, %title%, etc. anything in between % % gets treated as dynamic content'
            v-model='$1t.config.value.parseFilename'
        ></AdvancedSettingsToggle>
        <div v-if='$1t.config.value.parseFilename'>
            <q-input
                v-model='$1t.config.value.filenameTemplate'
                filled
                label='Filename scheme'
                class='input q-mb-sm' 
            ></q-input>
        </div>

        <!-- Move files -->
        <AdvancedSettingsToggle
            label='Move succesfully tagged files after tagging'
            v-model='$1t.config.value.moveSuccess'
        ></AdvancedSettingsToggle>
        <div v-if='$1t.config.value.moveSuccess'>
            <q-input
                v-model='$1t.config.value.moveSuccessPath'
                filled
                label='Target folder'
                class='input q-mb-sm' 
            ></q-input>
        </div>
        <AdvancedSettingsToggle
            label='Move files that failed to get tagged'
            v-model='$1t.config.value.moveFailed'
        ></AdvancedSettingsToggle>
        <div v-if='$1t.config.value.moveFailed'>
            <q-input
                v-model='$1t.config.value.moveFailedPath'
                filled
                label='Target folder'
                class='input q-mb-sm' 
            ></q-input>
        </div>

        <AdvancedSettingsToggle 
            label='Write .LRC file'
            tooltip='Write file with lyrics'
            v-model='$1t.config.value.writeLrc'
        ></AdvancedSettingsToggle>
        <AdvancedSettingsToggle 
            label='Enhanced LRC file'
            tooltip='Write per-word timestamps into LRC (if available). Not all players support this.'
            v-if='$1t.config.value.writeLrc'
            v-model='$1t.config.value.enhancedLrc'
        ></AdvancedSettingsToggle>

        <AdvancedSettingsToggle 
            label='Capitalize Genres'
            tooltip='Capitalize all of the genres'
            v-model='$1t.config.value.capitalizeGenres'
        ></AdvancedSettingsToggle>

        <!-- Album Tagging -->
        <AdvancedSettingsToggle
            label="Album Tagging"
            tooltip="Treat folders as albums"
            v-model='$1t.config.value.albumTagging'
        ></AdvancedSettingsToggle>
        <div class='row justify-center items-center q-px-md' v-if='$1t.config.value.albumTagging'>
            <div class='q-pb-lg'>How many % of tracks must be from same album to be treated as album: </div>
            <q-slider
                class='q-mt-sm'
                :min='0.0'
                :max='1.0'
                :step='0.02'
                v-model='$1t.config.value.albumTaggingRatio'
                label
                label-always
                :label-value="`${Math.round($1t.config.value.albumTaggingRatio  * 100)}%`"
            ></q-slider>
        </div>

        <!-- Duration -->   
        <div class='row justify-center items-center q-px-md'>
            <div>Match duration</div>
            <div class='text-grey-6 q-ml-sm'>
                <q-icon name='mdi-alert-circle-outline' class='q-mb-xs'></q-icon>
                Warning: Strict
            </div>
            <q-space></q-space>
            <q-toggle v-model='$1t.config.value.matchDuration'></q-toggle>
        </div>

    </q-list>

    <!-- Styles / genres action -->
    <q-separator class='q-mx-auto q-mt-md custom-sep-advanced' :style='"margin-top: 21px;"' inset color="dark"/>
    <div class='text-subtitle2 text-center text-bold text-primary q-mt-lg' :style='"margin-top: 35px;"'>
        GENRE / STYLE / SUBGENRE
        <div class='text-subtitle2 q-mb-md text-grey-6'>
            Fetch all <span class="text-grey-4">(default)</span>, if it should merge them, or write elsewhere <br><span class="text-grey-4">(supported platforms only)</span>
        </div>        
        
        <!-- Styles -->
        <q-select
            dark
            standout='text-grey-4 bg-dark'
            v-model='stylesOption'
            :options='stylesOptions'
            class='select'
            label='Genres/Styles tag'
            style='margin-bottom: 40px;'
            @update:model-value='updateStyleOption'
            popup-content-class='no-shadow'
        ></q-select>
        <!-- Styles custom tag -->
        <div v-if='$1t.config.value.stylesOptions == "customTag"'>
            <TagFields v-model='$1t.config.value.stylesCustomTag' class='q-mb-md'></TagFields>
        </div>
    </div>


    <!-- Multiple matches ordering -->
    <q-separator class='q-mx-auto q-mb-xl custom-sep-advanced' :style='"margin-top: 21px;"' inset color="dark"/>
    
    <q-select 
        dark
        standout='text-grey-4 bg-dark'
        class='row select q-ma-auto' :style='"margin-top: -8px;"'
        v-model='$1t.config.value.multipleMatches' 
        :options='multipleMatches'
        label='Multiple matches ordering'
        popup-content-class='no-shadow'
    ></q-select>    

    
    <!-- Strictness -->
    <div class='row justify-center q-mt-lg' style='max-width: 550px;'>
        <q-slider 
            v-model='$1t.config.value.strictness' 
            :min='0.0' 
            :max='1.0' 
            :step='0.05' 
            label 
            label-always
            class='slider q-mt-xl q-my-sm q-pb-lg col-10'
            label-text-color='black'
            :label-value='"Strictness: " + Math.round($1t.config.value.strictness*100) + "%"'
        >
        </q-slider>
        <q-icon name='mdi-help-circle-outline text-grey-6' class='q-pt-md q-mx-sm slider-tooltip'>
            <q-tooltip>
                The higher, the more accurate results, but less potential matches
            </q-tooltip>
        </q-icon>
    </div>

    <!-- Threads -->
    <div class='row justify-center' style='max-width: 550px;'>
        <q-slider 
            v-model='$1t.config.value.threads' 
            :min='1' 
            :max='24' 
            :step='1' 
            label 
            label-always
            class='slider q-mt-xl q-my-sm q-pb-lg col-10'
            label-text-color='black'
            :label-value='"Search Threads: " + $1t.config.value.threads'
        ></q-slider>
        <q-icon name='mdi-help-circle-outline text-grey-6' class='q-pt-md q-mx-sm slider-tooltip'>
            <q-tooltip>The higher, the faster, but uses more bandwidth, and not supported by all platforms</q-tooltip>
        </q-icon>
    </div>

    <!-- Title cleanup regex -->
    <q-input
        v-model='$1t.config.value.titleRegex'
        filled 
        label='Title cleanup regex'
        class='input'
    ></q-input>

    <!-- Cover filename -->
    <q-input
        v-model='$1t.config.value.coverFilename' 
        filled 
        label='Cover filename template'
        class='input monospace'
    ></q-input>

    
    <div class='row justify-center q-mt-sm' v-if='$1t.config.value.matchDuration'>
        <q-slider
            v-model='$1t.config.value.maxDurationDifference'
            :min='1'
            :max='60'
            :step='1'
            label
            label-always
            class='slider q-mt-lg'
            label-text-color='black'
            :label-value='"Max difference: " + $1t.config.value.maxDurationDifference + "s"'
        ></q-slider>
    </div>
    <br>

    

    <!-- Separators -->
    <q-separator class='q-mx-auto q-mt-md custom-sep-advanced' :style='"margin-top: 8px;"' inset color="dark"/>
    <div class='text-subtitle2 text-center text-bold text-primary q-mt-lg q-mb-sm' :style='"margin-top: 35px;"'>SEPARATORS</div>
    <div class='row q-pb-md q-mt-sm justify-center half-width'>
        <Separators v-model='$1t.config.value.separators'></Separators>
    </div>

    <!-- Track number padding -->
    
    <div class='text-subtitle2 text-center text-bold text-primary' style='margin-top: 3px;'>TAG OPTIONS</div>
    <div class='row q-pb-xs justify-center half-width'>
        <q-input 
            v-model.number='$1t.config.value.trackNumberLeadingZeroes' 
            filled 
            type='number'
            label='Track number leading zeroes'
            class='input'
        ></q-input>
    </div>

    <!-- ID3 Lang -->
    <div class='row q-pb-xs justify-center half-width'>
        <q-input :labelStyle="{ fontSize: '10px' }"
            v-model='$1t.config.value.id3CommLang' 
            filled 
            label='ID3 COMM Language'            
            class='input'
            :rules="[val => !val || val.length == 3]"
        ></q-input>
    </div>
    
    <q-separator class='q-mx-auto q-mb-lg custom-sep-advanced' style='margin-top: 16px;' inset color="dark"/>

    <!-- Command -->
    <div class='text-subtitle2 text-center text-bold text-primary q-mt-lg' style='margin-top: 35px;'>
        EXECUTE COMMAND WHEN TAGGING IS FINISHED
        <q-icon name='mdi-help-circle-outline text-grey-6' class='q-mx-sm q-mb-xs'>
            <q-tooltip>
                $success and $failed will be substituted with paths to files containing paths of affected tracks
            </q-tooltip>
        </q-icon>
    </div>
    <q-input label='$success, $failed will be substituted' filled class='row input q-py-sm justify-center' style='max-width: 526px; margin: auto;' v-model='$1t.config.value.postCommand'></q-input>
    <br>

    <br>
</div>
</template>

<script lang='ts' setup>
import { onMounted, ref } from 'vue';
import { get1t } from '../scripts/onetagger';
import { FrameName } from '../scripts/utils';

import Separators from './Separators.vue';
import AdvancedSettingsToggle from './AdvancedSettingsToggle.vue';
import TagFields from './TagFields.vue';
import { SUPPORTED_TAGS, SupportedTag } from '../scripts/autotagger';

const $1t = get1t();
const multipleMatches = ['Default', 'Oldest', 'Newest'];
const stylesOptions = ["Default", "Only Genre(s)", "Only Style(s)", "Merge to Genre tag", 
    "Merge to Style tag", "Write Style to Genre tag", "Write Genre to Style tag",
    "Write Style to Custom tag"];
const values = ["default", "onlyGenres", "onlyStyles", "mergeToGenres", "mergeToStyles",
    "stylesToGenre", "genresToStyle", "customTag"];
const stylesOption = ref('Default');

function updateStyleOption() {
    $1t.config.value.stylesOptions = values[stylesOptions.indexOf(stylesOption.value)];
}

onMounted(() => {
    stylesOption.value = stylesOptions[values.indexOf($1t.config.value.stylesOptions)];
    // In case of null because of update
    if (!$1t.config.value.stylesCustomTag)
        $1t.config.value.stylesCustomTag = FrameName.same('STYLE');
});
</script>

<style>
.slider-tooltip {
    margin-top: 36px;
}

.half-width {
    max-width: 50vw;
}
</style>

<style scoped>
.list {
    width: 36%; 
    margin-left: 32%; 
    text-align: left;
}

.custom-sep-advanced {
    min-width: 600px;
    margin-left: -75px;    
}
</style>