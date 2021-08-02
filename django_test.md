# Tests

### Sample test on model

    from django.test import TestCase
    from django.contrib.auth import get_user_model
    from .models import Hike

    class HikeTest(TestCase):

        @classmethod
        def setUpTestData(cls):
            testuser1 = get_user_model().objects.create_user(username='testuser1', password='pass')
            testuser1.save()

            test_hike = Hike.objects.create(
                author = testuser1,
                trail_name = "Paintbrush Divide to Cascade Canyon, Grand Teton",
                description = "Unbelievable trip!"
            )

            test_hike.save()

        def test_hike_content(self):
            hike = Hike.objects.get(id=1)
            actual_author = str(hike.author)
            actual_trail_name = hike.trail_name
            actual_description = hike.description

            expected_author = 'testuser1'
            expected_trail_name = "Paintbrush Divide to Cascade Canyon, Grand Teton"
            expected_description = "Unbelievable trip!"

            self.assertEqual(actual_author, expected_author)
            self.assertEqual(actual_trail_name, expected_trail_name)
            self.assertEqual(actual_description, expected_description)
