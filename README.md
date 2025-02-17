class Page(models.Model):
    name=models.CharField(default='My default page',max_length=200,blank=False)
    created_at=models.DateTimeField(auto_now_add=True)
    owner=models.ForeignKey(User,on_delete=models.CASCADE)
    slug=models.SlugField()
    uuid=models.UUIDField(default=uuid.uuid4, editable=False)
    is_public=models.BooleanField(default=False)
    def __str__(self):  
        return self.name
    class Meta:
        ordering=['position','created_at']
@receiver(post_save, sender=Page)
def create_shared_page_entry(sender,instance,created,kwargs):
    if created:
        shared_page=SharedPage.objects.create(page=instance,user=instance.user,can_edit=True)
