<template>
  <div class="submenu rounded-4 p-3 h-100 d-flex flex-column">
    <div v-if="category === 'notes'" class="d-flex flex-column flex-grow-1">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h6 class="mb-0 fw-bold">My notes</h6>
        <button
          class="btn btn-sm btn-outline-primary rounded-pill"
          @click="addNote"
        >
          + Add
        </button>
      </div>

      <div class="notes-list">
        <ul class="list-unstyled mb-0">
          <li
            v-for="note in notes"
            :key="note.id"
            class="note-item d-flex align-items-center gap-2 mb-2"
            :class="{ active: 'note-' + note.id === activeSub }"
            @click="$emit('select-sub', 'note-' + note.id)"
          >
            <input
              type="checkbox"
              class="form-check-input"
              v-model="note.done"
              @click.stop="note.done = !note.done"
            />
            <span
              class="priority-dot"
              :class="'priority-' + note.priority"
            ></span>
            <div class="flex-grow-1">
              <div class="fw-semibold small">{{ note.title }}</div>
              <div class="text-muted xsmall">{{ note.priority }} priority</div>
            </div>
            <button
              class="btn btn-sm p-0 px-1 note-delete-btn"
              @click.stop="deleteNote(note.id)"
            >
              <img :src="trash" alt="delete" class="note-delete-icon" />
            </button>
          </li>
        </ul>
      </div>
    </div>

    <div
      v-else-if="category === 'guests'"
      class="mt-4 p-3 bg-white rounded-4 shadow-sm"
    >
      <h6 class="fw-bold mb-2 text-center">Guests overview</h6>

      <div class="progress rounded-pill mb-2" style="height: 18px">
        <div
          class="progress-bar bg-navy"
          role="progressbar"
          :style="{ width: guestProgress + '%' }"
          :aria-valuenow="guestProgress"
          aria-valuemin="0"
          aria-valuemax="100"
        ></div>
      </div>

      <div class="mt-2 small text-muted">
        <div>
          Invited (people): <strong>{{ guestStats.totalSeats }}</strong>
        </div>
        <div>
          Confirmed: <strong>{{ guestStats.confirmedSeats }}</strong>
        </div>
        <div>
          Maybe: <strong>{{ guestStats.maybeSeats }}</strong>
        </div>
        <div>
          Declined: <strong>{{ guestStats.declinedSeats }}</strong>
        </div>
        <div>
          Pending: <strong>{{ guestStats.pendingSeats }}</strong>
        </div>
      </div>
    </div>
    <div v-else-if="category === 'chat'" class="d-flex flex-column flex-grow-1">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <h6 class="mb-0 fw-bold">Chats</h6>
        <button
          class="btn btn-sm btn-outline-primary rounded-pill"
          type="button"
        >
          + New
        </button>
      </div>

      <div class="chat-list">
        <ul class="list-unstyled mb-0">
          <li
            v-for="chat in chats"
            :key="chat.id"
            class="chat-item d-flex align-items-center gap-2 mb-2"
            :class="{ active: 'chat-' + chat.id === activeSub }"
            @click="$emit('select-sub', 'chat-' + chat.id)"
          >
            <div
              class="chat-avatar rounded-circle d-flex align-items-center justify-content-center"
            >
              <span>{{ getChatInitials(chat.title) }}</span>
            </div>

            <div class="flex-grow-1">
              <div class="d-flex justify-content-between align-items-center">
                <span class="fw-semibold small">{{ chat.title }}</span>
              </div>
              <div class="xsmall text-muted text-truncate">
                {{ chat.subjectType || "Chat" }}
              </div>
            </div>

            <button
              class="btn btn-sm p-0 px-1 chat-delete-btn"
              @click.stop="$emit('delete-chat', chat.id)"
            >
              <img :src="trash" alt="delete" class="chat-delete-icon" />
            </button>
          </li>
        </ul>
      </div>
    </div>

    <ul v-else class="list-unstyled mb-0">
      <li
        v-for="item in items"
        :key="item.id"
        class="sub-item d-flex align-items-center gap-3 mb-2"
        :class="{ active: item.label === activeSub }"
        @click="$emit('select-sub', item.label)"
        role="button"
      >
        <img :src="item.icon" alt="" class="icon-img" />
        <span>{{ item.label }}</span>
      </li>
    </ul>

    <div
      v-if="category === 'budget'"
      class="mt-4 p-3 bg-white rounded-4 shadow-sm text-center"
    >
      <h6 class="fw-bold mb-2">Budget Overview</h6>
      <div class="progress rounded-pill" style="height: 18px">
        <div
          class="progress-bar bg-navy"
          role="progressbar"
          :style="{ width: progress + '%' }"
          :aria-valuenow="progress"
          aria-valuemin="0"
          aria-valuemax="100"
        ></div>
      </div>
      <div class="mt-2 small text-muted">
        <div>
          Planned: <strong>{{ budgetPlanned }} €</strong>
        </div>
        <div>
          Spent: <strong>{{ budgetSpent }} €</strong>
        </div>
        <div>
          Remaining: <strong>{{ remaining }} €</strong>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import bride from "@/assets/bride.png";
import ring from "@/assets/ring.png";
import hairdresser from "@/assets/hairdresser.png";
import heels from "@/assets/heels.png";
import nails from "@/assets/nails.png";
import maid from "@/assets/maid.png";
import tai from "@/assets/tai.png";
import groom from "@/assets/groom.png";
import shirt from "@/assets/shirt.png";
import suit from "@/assets/suit.png";
import shoes from "@/assets/shoes.png";
import car from "@/assets/car.png";
import barber from "@/assets/barber.png";
import time from "@/assets/time.png";
import church from "@/assets/church.png";
import flowers from "@/assets/flowers.png";
import lapel from "@/assets/lapel.png";
import bouquet from "@/assets/bouquet.png";
import engagement from "@/assets/engagement.png";
import music from "@/assets/music.png";
import priest from "@/assets/priest.png";
import note from "@/assets/note.png";
import readers from "@/assets/readers.png";
import confetti from "@/assets/confetti.png";
import band from "@/assets/band.png";
import menu from "@/assets/menu.png";
import halla from "@/assets/halla.png";
import cake from "@/assets/cake.png";
import dance from "@/assets/dance.png";
import trash from "@/assets/trash.png";

export default {
  name: "SubMenu",
  props: {
    category: String,
    activeSub: String,
    notes: {
      type: Array,
      default: () => [],
    },
    activeNoteId: Number,
    guests: {
      type: Array,
      default: () => [],
    },
    chats: {
      type: Array,
      default: () => [],
    },
    budgetPlanned: {
      type: Number,
      default: 0,
    },
    budgetSpent: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      notes: [
        {
          id: 1,
          title: "Checklist for photographer",
          priority: "high",
          done: false,
        },
        {
          id: 2,
          title: "Things to ask the hall",
          priority: "medium",
          done: true,
        },
        { id: 3, title: "Guests to call", priority: "low", done: false },
      ],
      trash,
    };
  },
  computed: {
    items() {
      const map = {
        bride: [
          {
            label: "Wedding dress",
            icon: bride,
          },
          { label: "Rings", icon: ring },
          { label: "Shoes", icon: heels },
          { label: "Hairdresser", icon: hairdresser },
          { label: "Nails", icon: nails },
          {
            label: "Maid of Honor",
            icon: maid,
          },
        ],
        groom: [
          { label: "Tie", icon: tai },
          { label: "Shoes", icon: shoes },
          { label: "Shirt", icon: shirt },
          { label: "Suit", icon: suit },
          { label: "Hairdresser", icon: hairdresser },
          { label: "Barber", icon: barber },
          { label: "Best Man", icon: groom },
        ],
        flowers: [
          { label: "Bouquet", icon: bouquet },
          { label: "Lapel", icon: lapel },
          {
            label: "Car flowers",
            icon: car,
          },
          { label: "Hall flowers", icon: flowers },
          {
            label: "Church flowers",
            icon: church,
          },
        ],
        church: [
          { label: "Date & Time", icon: time },
          {
            label: "Engagement course",
            icon: engagement,
          },
          { label: "Music", icon: music },
          { label: "A Priest", icon: priest },
          { label: "Readers", icon: readers },
          { label: "Documents", icon: note },
          { label: "Confetti", icon: confetti },
        ],
        hall: [
          { label: "Hall", icon: halla },
          { label: "Menu", icon: menu },
          { label: "Cake", icon: cake },
          { label: "Band", icon: band },
          { label: "First dance", icon: dance },
        ],
      };
      return map[this.category] || [];
    },
    remaining() {
      return this.budgetPlanned - this.budgetSpent;
    },
    progress() {
      if (!this.budgetPlanned) return 0;
      return Math.min((this.budgetSpent / this.budgetPlanned) * 100, 100).toFixed(0);
    },
    guestStats() {
      const totalInvited = this.guests.length;

      const totalSeats = this.guests.reduce(
        (sum, g) => sum + (g.seats || 1),
        0
      );

      const confirmedSeats = this.guests
        .filter((g) => g.status === "confirmed")
        .reduce((sum, g) => sum + (g.seats || 1), 0);

      const declinedSeats = this.guests
        .filter((g) => g.status === "declined")
        .reduce((sum, g) => sum + (g.seats || 1), 0);

      const maybeSeats = this.guests
        .filter((g) => g.status === "maybe")
        .reduce((sum, g) => sum + (g.seats || 1), 0);

      const pendingSeats =
        totalSeats - confirmedSeats - declinedSeats - maybeSeats;

      return {
        totalInvited,
        totalSeats,
        confirmedSeats,
        declinedSeats,
        maybeSeats,
        pendingSeats,
      };
    },

    guestProgress() {
      return this.guestStats.totalSeats
        ? Math.min(
            (this.guestStats.confirmedSeats / this.guestStats.totalSeats) * 100,
            100
          ).toFixed(0)
        : 0;
    },
  },
  methods: {
    getChatInitials(title) {
      if (!title) return "?";
      const words = title.split(" ").filter((w) => w.length > 0);
      if (words.length === 1) {
        return words[0].substring(0, 2).toUpperCase();
      }
      return (words[0][0] + words[words.length - 1][0]).toUpperCase();
    },
    addNote() {
      const id = this.notes.length
        ? Math.max(...this.notes.map((n) => n.id)) + 1
        : 1;
      this.notes.push({
        id,
        title: "New note",
        priority: "low",
        done: false,
      });
      this.$emit("select-sub", "note-" + id);
    },
    deleteNote(id) {
      this.notes = this.notes.filter((n) => n.id !== id);
      if ("note-" + id === this.activeSub) {
        const first = this.notes[0];
        this.$emit("select-sub", first ? "note-" + first.id : "");
      }
    },
  },
};
</script>

<style scoped>
.submenu {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: calc(100vh - 200px);
}
.sub-item {
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 0.8rem;
  border-radius: 999px;
  transition: 0.2s;
  display: flex;
  align-items: center;
  gap: 0.75rem;
}
.sub-item.active,
.sub-item:hover {
  background: #d4af37;
  color: #fff;
}
.icon-img {
  width: 52px;
  height: 52px;
  flex: 0 0 52px;
  object-fit: contain;
  filter: invert(16%) sepia(89%) saturate(894%) hue-rotate(199deg)
    brightness(90%) contrast(90%);
}
.progress {
  background-color: rgba(0, 0, 0, 0.1);
}

.bg-navy {
  background-color: rgb(13, 23, 80);
}

.note-item {
  background: rgba(255, 255, 255, 0.5);
  padding: 0.4rem 0.6rem;
  border-radius: 0.9rem;
  transition: 0.15s;
  cursor: pointer;
}
.note-item.active,
.note-item:hover {
  background: #d4af37;
  color: #fff;
}
.priority-dot {
  width: 14px;
  height: 14px;
  border-radius: 999px;
}
.priority-high {
  background: #dc3545;
}
.priority-medium {
  background: #ffc107;
}
.priority-low {
  background: #198754;
}
.xsmall {
  font-size: 0.65rem;
}

.note-delete-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}
.note-delete-btn {
  background: transparent;
  border: none;
}
.chat-item {
  background: rgba(255, 255, 255, 0.5);
  padding: 0.4rem 0.6rem;
  border-radius: 0.9rem;
  transition: 0.15s;
  cursor: pointer;
}
.chat-item.active,
.chat-item:hover {
  background: #d4af37;
  color: #fff;
}
.chat-avatar {
  width: 36px;
  height: 36px;
  background: rgba(13, 23, 80, 0.1);
  font-size: 0.8rem;
  font-weight: 600;
}

.chat-delete-icon {
  width: 18px;
  height: 18px;
  object-fit: contain;
}

.chat-delete-btn {
  background: transparent;
  border: none;
}

.submenu {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: 0;
  display: flex;
  flex-direction: column;
}

.notes-list,
.chat-list {
  flex: 1 1 auto;
  min-height: 0;
  max-height: calc(100vh - 260px);
  overflow-y: auto;
}
</style>
