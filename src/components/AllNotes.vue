<template>
	<section class="all-notes">
		
		<NewNote/>

		<div class="all-notes__empty" v-if="arrNotes.length == 0">
			<i class="fab fa-think-peaks"></i><br>
			<h2>Here is thoughts placeholder.</h2>
			<h3>Notes that you add appear here.</h3>
		</div>

		<p v-if="arrPinned.length != 0" class="mb-1">PINNED</p>
		<div v-if="arrPinned.length != 0" class="grid-allnotes m-auto">
			<article class="note mb-2" v-for="notePinned in arrPinned" :key="notePinned.id" >
				<form>
					<div @click="onOpenNote($event, note)">
						<div class="note__title-container mb-1">
							<div 
								class="note__title"
								contenteditable="true"
								data-placeholder="Title"
								v-text="notePinned.title"
								@blur="onEditTitle"
								ref="note__title">
							</div>
							<i class="fas fa-thumbtack note__pin" :class="{ 'isPinned': isPinned }" title="Pin note"></i>
						</div>


						<div
							class="note__body"
							ref="note__body"
							contenteditable="true"
							data-placeholder="Take a note..."
							v-text="notePinned.body"
							@blur="onEditBody">
						</div>
					</div>

					<h4 class="note__tag mt-3" v-if="notePinned.tags">{{ notePinned.tags }}</h4>

					<div class="note__options mt-1" :class="{ 'open-note-opacity': openNote }">
						<i class="fas fa-tags" title="Add tag"></i>
						<div class="note__pallete">
							<i class="fas fa-palette" title="Change color"></i>

							<div class="note__colors-container">
								<div class="color-1 red"></div>
								<div class="color-2 blue"></div>
								<div class="color-3 green"></div>
								<div class="color-4 purple"></div>
								<div class="color-5 orange"></div>
								<div class="color-6 grey"></div>
							</div>
						</div>
						<i class="fas fa-archive" title="Archive" @click="onArchiveNote(notePinned)"></i>
						<i class="fas fa-trash" title="Delete" @click="onDeleteNote(notePinned)"></i>

						<button class="btn" @click.prevent="onCloseNote" v-if="openNote">close</button>
					</div>
				</form>
			</article>
		</div>

		<br><hr v-if="arrPinned.length != 0"><br>
		
<!-- ======================================= ALL NOTES ====================================== -->

		<div v-if="arrNotes.length != 0" class="grid-allnotes m-auto">
			<article class="note mb-2" v-for="note in arrNotes" :key="note.id" >
				<form>
					<div class="note__click-open-note" @click="onOpenNote($event, note)">
						<div class="note__title-container mb-1">
							<div 
								class="note__title"
								contenteditable="true"
								data-placeholder="Title"
								v-text="note.title"
								@blur="onEditTitle"
								ref="note__title">
							</div>
							<i class="fas fa-thumbtack note__pin" :class="{ 'open-note-opacity': openNote }" title="Pin note"></i>
						</div>


						<div
							class="note__body"
							ref="note__body"
							contenteditable="true"
							data-placeholder="Take a note..."
							v-text="note.body"
							@blur="onEditBody">
						</div>
					</div>

					<h4 class="note__tag mt-3" v-if="note.tags">{{ note.tags }}</h4>

					<div class="note__options mt-1" :class="{ 'open-note-opacity': openNote }">
						<i class="fas fa-tags" title="Add tag"></i>
						<div class="note__pallete">
							<i class="fas fa-palette" title="Change color"></i>

							<div class="note__colors-container">
								<div class="color-1 red"></div>
								<div class="color-2 blue"></div>
								<div class="color-3 green"></div>
								<div class="color-4 purple"></div>
								<div class="color-5 orange"></div>
								<div class="color-6 grey"></div>
							</div>
						</div>
						<i class="fas fa-archive" title="Archive" @click="onArchiveNote(note)"></i>
						<i class="fas fa-trash" title="Delete" @click="onDeleteNote(note)"></i>

						<button class="btn" @click.prevent="onCloseNote" v-if="openNote">close</button>
					</div>
				</form>
			</article>
		</div>


	<div class="open-note-background" v-if="openNoteBackground" @click="onCloseNote"></div>
	</section>
</template>

<script>
import NewNote from '@/components/NewNote.vue'

export default {
	components: {
		NewNote
	},

	data() {
		return {
			noNotes: true,
			noteTitle: '',
			noteBody: '',
			openNote: false,
			openNoteBackground: false,
			currNote: null,
			isPinned: false,
			arrPinned: [],
			arrTag: [],
			arrArchive: [],
			arrDeleted: [],
			arrColors: ['red, blue, green, purple, orange, grey'],
			arrNotes: [
				{
					id: 1,
					title: 'Neki naslov',
					body: 'Quidem, eum. Sed veniam fugit sequi quisquam? Dignissimos quo nemo suscipit?',
					pinned: false,
					color: 'red',
					date: 'Sep 11 2019, 12:45AM',
					// tags: 'it'
				},

				{
					id: 2,
					title: 'Patek',
					body: 'Quidem, eum. Sed veniam fugit sequi quisquam? Dignissimos quo nemo suscipit? Upiditate ullam numquam nemo aut',
					pinned: true,
					color: 'white',
					date: 'Sep 11 2019, 12:45AM',
					// tags: 'mile'
				},

				{
					id: 3,
					title: 'Neki naslov',
					body: 'Quidem, eum. Sed veniam fugit sequi quisquam? Dignissimos quo nemo suscipit? Dignissimos quo nemo suscipit?Dignissimos quo nemo suscipit?',
					pinned: true,
					color: 'orange',
					date: 'Sep 11 2019, 12:45AM',
					tags: 'rodjendan'
				},

				{
					id: 4,
					title: 'Neki naslov',
					body: 'Quidem, eum. Sed veniam fugit sequi quisquam? Dignissimos quo nemo suscipit?Dignissimos quo nemo suscipit?Dignissimos quo nemo suscipit? Dignissimos quo nemo suscipit? Dignissimos quo nemo suscipit?',
					pinned: false,
					color: 'green',
					date: 'Sep 11 2019, 12:45AM',
					tags: 'party'
				}
			]
		}
	},

	// watch: {
	// 	// this.noteTitle = this.$refs.note__title.innerText
	// 	// this.noteBody = this.$refs.note__body.innerText
	// 	// console.log(this.noteTitle, this.noteBody);

	// 	innerTitle(newValue) {
	// 		// if (document.activeElement == this.$el) {
    //         //     return;
    //         // }
	// 		// this.$el.innerText = newValue
	// 	},

	// 	innerBody(newValue) {

	// 	}
	// },

	methods: {
		onEditTitle(e) {
			this.noteTitle = e.target.innerText
			// this.noteTitle = value
			// console.log(this.noteTitle);
		},

		onEditBody(e) {
			this.noteBody = e.target.innerText
			// this.noteBody = value
		},

		// onClose() {
		// 	this.showBody = false
		// 	this.noteTitle = ''
		// 	this.noteBody = ''
		// },

		onOpenNote(e, note) {
			if(e.target.classList.contains('note__pin')) {
				this.pinNote(note)
			} else {
				this.currNote = e.target.closest('.note')
				this.currNote.classList.add('open-note')
				this.openNoteBackground = true
				this.openNote = true
			}
		},

		onCloseNote() {
			if(this.currNote.classList.contains('open-note')) {
				this.currNote.classList.remove('open-note')
			}
			this.openNoteBackground = false
			this.openNote = false
		},

		onAddTag() {

		},

		onAddColor() {

		},

		removeFromAllNotes(note) {
			this.arrNotes = this.arrNotes.filter(currNote => currNote.id != note.id)
		},

		removeFromArray(note, arr) {
			arr = arr.filter(currNote => currNote.id != note.id)
		},
		addToArray(note, arr) {
			arr.unshift(note)
		},

		onArchiveNote(note) {
			this.arrArchive.unshift(note)
			// this.arrNotes = this.arrNotes.filter(currNote => currNote.id != note.id)
			this.removeFromAllNotes(note)
			// this.addToArray(note, this.arrArchive)
			// this.removeFromArray(note, this.arrNotes)
			console.log(this.arrArchive)
			this.onCloseNote()
		},

		onDeleteNote(note) {
			this.arrDeleted.unshift(note)
			// this.arrNotes = this.arrNotes.filter(currNote => currNote.id != note.id)
			this.removeFromAllNotes(note)
			// this.addToArray(note, this.arrDeleted)
			// this.removeFromArray(note, this.arrNotes)
			this.onCloseNote()
		},

		// onPinNote(note) {
		// 	this.arrPinned.unshift(note)
		// 	// this.arrNotes = this.arrNotes.filter(currNote => currNote.id != note.id)
		// 	this.removeFromAllNotes(note)
		// 	this.arrNotes.map(currNote => {
		// 		if(currNote.id == note.id) {
		// 			currNote.pinned = true
		// 		}
		// 	})
		// 	console.log(this.arrNotes);
		// }

		pinNote(note) {
			this.arrPinned.unshift(note)
			this.removeFromAllNotes(note)
			// this.addToArray(note, this.arrPinned)
			// this.removeFromArray(note, this.arrNotes)
			this.arrNotes.map(currNote => {
				if(currNote.id == note.id) {
					currNote.pinned = true
				}
			})
			this.onCloseNote()
			console.log(this.arrNotes);
		}
	} 
}
</script>