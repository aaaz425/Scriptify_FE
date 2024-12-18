<template>
  <div class="review-board">
    <div v-for="review in reviews" :key="review.reviewId" class="review-item">
      <div class="review-left">
        <strong>{{ review.userName }}</strong>
      </div>
      <div class="review-center">
        <div class="stars">
          <span v-for="star in 5" :key="star" class="star">
            {{ star <= review.rank ? "★" : "☆" }}
          </span>
        </div>
      </div>
      <div class="review-right">
        <p>{{ review.content }}</p>
      </div>

      <div v-if="review.userId == authStore.userId" class="review-actions">
        <button class="update-btn" @click="editReview(review)">수정</button>
        <button class="delete-btn" @click="deleteReview(review.reviewId)">
          삭제
        </button>
      </div>
      <div v-else class="review-actions">
        <button class="fake-btn">삭제</button>
        <button class="fake-btn">수정</button>
      </div>

      <ReviewUpdateForm
        v-if="updateReviewId === review.reviewId && showUpdateBox"
        @update-review="updateReview"
        @cancel-edit="cancelEdit"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import ReviewUpdateForm from "./ReviewUpdateForm.vue";
import { useReviewStore } from "@/stores/review";
import { useAuthStore } from "@/stores/auth";

defineProps({
  reviews: {
    type: Array,
    required: true,
    default: () => [],
  },
});

const emit = defineEmits(["update-review", "delete-review"]);

const reviewStore = useReviewStore();
const authStore = useAuthStore();
const showUpdateBox = ref(false);
const updateReviewId = ref(null);

const editReview = (review) => {
  updateReviewId.value = review.reviewId;
  showUpdateBox.value = true;
};

async function deleteReview(reqReviewId) {
  showUpdateBox.value = false;

  const request = {
    reviewId: reqReviewId,
  };

  try {
    await reviewStore.deleteReview(request);

    emit("delete-review", reqReviewId);
  } catch (error) {
    console.error("Error while handling message:", error);
  }
}

async function updateReview(reviewDto) {
  showUpdateBox.value = false;

  const request = {
    userId: authStore.userId,
    reviewId: updateReviewId.value,
    rank: reviewDto.rank,
    content: reviewDto.content,
  };

  try {
    await reviewStore.updateReview(request);

    emit("update-review", request, updateReviewId.value);

    updateReviewId.value = null;
  } catch (error) {
    console.error("Error while handling message:", error);
  }
}

const cancelEdit = () => {
  showUpdateBox.value = false;
};
</script>

<style scoped>
.review-board {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.review-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 50%;
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  font-family: "Jua", sans-serif;
  font-weight: 400;
  font-style: normal;
  height: 30px;
}

.review-left {
  flex: 1;
  text-align: left;
}

.review-center {
  flex: 1;
  text-align: center;
}

.stars {
  font-size: 20px;
  color: gold;
}

.review-right {
  flex: 2;
  text-align: left;
}
.update-btn {
  text-decoration: none;
  color: black;
  border: none;
  background-color: inherit;
  font-size: 15px;
}

.delete-btn {
  text-decoration: none;
  color: black;
  border: none;
  background-color: inherit;
  font-size: 15px;
}

.update-btn:hover,
.delete-btn:hover {
  cursor: pointer;
}

.fake-btn {
  color: #f9f9f9;
  background-color: inherit;
  border: none;
}
</style>
