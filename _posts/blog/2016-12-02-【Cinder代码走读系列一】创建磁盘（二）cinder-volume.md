---
layout: post
title: 【OpenStack源码单排 一】创建磁盘 create_volume.py走读
categories: blog
---
*声明：本博客欢迎转发，但请保留原作者信息! 内容系本人学习、研究和总结，如有雷同，实属荣幸！*
 
*新浪微博：[@支支zHi小冬](http://weibo.com/u/1596536485/home?wvr=5)*

*博客地址：[http://xiaodongzhi.github.io/](http://xiaodongzhi.github.io/)*

*联系邮箱：517341003@qq.com*

 
openstack版本信息：ocata


/opt/stack/cinder/cinder/volume/flows/manager/create_volume.py(809)execute()

```python
def execute(self, context, volume, volume_spec):
入参说明：
volume_spec:
{'status': u'creating', 'image_location': (None, None), 'image_id': '801a7b53-1e25-4f49-b0f3-d2dd61a9f388', 'volume_id': 'eddfec51-9bc5-4649-8050-96ef7b909b30', 'volume_size': 10, 'image_service': <cinder.image.glance.GlanceImageService object at 0x7f6d37f6a710>, 'image_meta': {'status': u'active', 'name': u'VSA', 'deleted': False, 'container_format': u'bare', 'created_at': datetime.datetime(2016, 12, 5, 3, 38, 1, tzinfo=<iso8601.Utc>), 'disk_format': u'vhd', 'updated_at': datetime.datetime(2016, 12, 5, 3, 49, 16, tzinfo=<iso8601.Utc>), 'id': u'801a7b53-1e25-4f49-b0f3-d2dd61a9f388', 'owner': u'cfa52d30378146cbaabb4670cc6f0786', 'protected': False, 'min_ram': 0, 'checksum': u'99396230fb8b6617b8784173ac3d0727', 'min_disk': 0, 'is_public': True, 'deleted_at': None, 'properties': {u'os-type': u'Linux', u'os_distro': u'Novell SUSE Linux Enterprise Server 11 SP3 64bit', u'os_type': u'Linux'}, 'size': 787726336}, 'volume_name': 'volume-eddfec51-9bc5-4649-8050-96ef7b909b30', 'type': 'image'}

volume:
Volume(_name_id=None,admin_metadata={},attach_status='detached',availability_zone='nova',bootable=False,cluster=<?>,cluster_name=None,consistencygroup=<?>,consistencygroup_id=None,created_at=2016-12-05T10:05:25Z,deleted=False,deleted_at=None,display_description=None,display_name='zd2',ec2_id=None,encryption_key_id=None,glance_metadata=<?>,group=<?>,group_id=None,host='jenkins@DS01#DS02',id=eddfec51-9bc5-4649-8050-96ef7b909b30,launched_at=None,metadata={},migration_status=None,multiattach=False,previous_status=None,project_id='cfa52d30378146cbaabb4670cc6f0786',provider_auth=None,provider_geometry=None,provider_id=None,provider_location=None,replication_driver_data=None,replication_extended_status=None,replication_status='disabled',scheduled_at=2016-12-05T10:05:25Z,size=10,snapshot_id=None,snapshots=<?>,source_volid=None,status='creating',terminated_at=None,updated_at=2016-12-05T10:05:25Z,user_id='53f34ce4a29f4c6ebf1d57957af6986a',volume_attachment=<?>,volume_type=VolumeType(5c718eb8-4556-4d89-9edc-31738cd0c848),volume_type_id=5c718eb8-4556-4d89-9edc-31738cd0c848)
context.__dict__
{'domain': None, 'project_name': u'admin', 'project_domain': u'default', 'timestamp': datetime.datetime(2016, 12, 5, 10, 5, 24, 921988, tzinfo=<FixedOffset u'+00:00' datetime.timedelta(0)>), 'auth_token': u'gAAAAABYRTvkecCacYonziumD64QPkqMZYcgOcbqu7AP2LbiXNmcEh_VSuHlobemeTofZdbq4coe0wzURI8-8hFz5i1IhWNjrEWnH_mZtFnqGPBcnMO7XvMBvhhxIjyXqNHkcNhrsM8uUA6K2T8nJXEubNKKwzULbVFjcMkVItHYFLKjjI0gZ94', 'remote_address': u'10.100.7.17', 'quota_class': None, 'resource_uuid': None, 'is_admin': True, 'user': u'53f34ce4a29f4c6ebf1d57957af6986a', 'service_catalog': [{u'endpoints': [{u'adminURL': u'http://compute.az1.dc1.RegionOne.com:8774/v2/cfa52d30378146cbaabb4670cc6f0786', u'region': u'RegionOne', u'internalURL': u'http://compute.az1.dc1.RegionOne.com:8774/v2/cfa52d30378146cbaabb4670cc6f0786', u'publicURL': u'http://compute.az1.dc1.RegionOne.com:8774/v2/cfa52d30378146cbaabb4670cc6f0786'}], u'type': u'compute', u'name': u'nova'}, {u'endpoints': [{u'adminURL': u'http://identity.az1.dc1.RegionOne.com:35357/v2.0', u'region': u'RegionOne', u'internalURL': u'http://10.100.7.17/identity', u'publicURL': u'http://10.100.7.17/identity'}], u'type': u'identity', u'name': u'keystone'}, {u'endpoints': [{u'adminURL': u'http://10.100.7.17:9292', u'region': u'RegionOne', u'internalURL': u'http://image.az1.dc1.RegionOne.com:9292/', u'publicURL': u'http://10.100.7.17:9292'}], u'type': u'image', u'name': u'glance'}], 'tenant': u'cfa52d30378146cbaabb4670cc6f0786', 'read_only': False, 'show_deleted': False, 'roles': [u'admin'], 'domain_name': None, 'is_admin_project': True, '_read_deleted': u'no', 'request_id': u'req-dd78bc47-ca01-45c0-b789-a204e2cc2a61', 'user_domain': u'default', 'user_name': None, 'user_domain_name': None, 'project_domain_name': None}

	关键流程如下：

    判断创建类型，根据不同创建类型调用不同的创建方法、create_type的数据来源于volume_spec中的“type”字段
    if create_type == 'raw':
        model_update = self._create_raw_volume(volume, **volume_spec)
    elif create_type == 'snap':
        model_update = self._create_from_snapshot(context, volume,
                                                  **volume_spec)
    elif create_type == 'source_vol':
        model_update = self._create_from_source_volume(
            context, volume, **volume_spec)
    elif create_type == 'source_replica':
        model_update = self._create_from_source_replica(
            context, volume, **volume_spec)
    elif create_type == 'image':
        model_update = self._create_from_image(context,
                                               volume,
                                               **volume_spec)
	





此处是根据镜像创建，则调用：

def _create_from_image(self, context, volume,
			   image_location, image_id, image_meta,
			   image_service, **kwargs):
其中入参：
volume：
 Volume(_name_id=None,admin_metadata={},attach_status='detached',availability_zone='nova',bootable=False,cluster=<?>,cluster_name=None,consistencygroup=<?>,consistencygroup_id=None,created_at=2016-12-05T10:05:25Z,deleted=False,deleted_at=None,display_description=None,display_name='zd2',ec2_id=None,encryption_key_id=None,glance_metadata=<?>,group=<?>,group_id=None,host='jenkins@DS01#DS02',id=eddfec51-9bc5-4649-8050-96ef7b909b30,launched_at=None,metadata={},migration_status=None,multiattach=False,previous_status=None,project_id='cfa52d30378146cbaabb4670cc6f0786',provider_auth=None,provider_geometry=None,provider_id=None,provider_location=None,replication_driver_data=None,replication_extended_status=None,replication_status='disabled',scheduled_at=2016-12-05T10:05:25Z,size=10,snapshot_id=None,snapshots=<?>,source_volid=None,status='creating',terminated_at=None,updated_at=2016-12-05T10:05:25Z,user_id='53f34ce4a29f4c6ebf1d57957af6986a',volume_attachment=<?>,volume_type=VolumeType(5c718eb8-4556-4d89-9edc-31738cd0c848),volume_type_id=5c718eb8-4556-4d89-9edc-31738cd0c848)

image_id：
 '801a7b53-1e25-4f49-b0f3-d2dd61a9f388'
image_location：
 (None, None)
image_meta：
 {'status': u'active', 'name': u'VSA', 'deleted': False, 'container_format': u'bare', 'created_at': datetime.datetime(2016, 12, 5, 3, 38, 1, tzinfo=<iso8601.Utc>), 'disk_format': u'vhd', 'updated_at': datetime.datetime(2016, 12, 5, 3, 49, 16, tzinfo=<iso8601.Utc>), 'id': u'801a7b53-1e25-4f49-b0f3-d2dd61a9f388', 'owner': u'cfa52d30378146cbaabb4670cc6f0786', 'protected': False, 'min_ram': 0, 'checksum': u'99396230fb8b6617b8784173ac3d0727', 'min_disk': 0, 'is_public': True, 'deleted_at': None, 'properties': {u'os-type': u'Linux', u'os_distro': u'Novell SUSE Linux Enterprise Server 11 SP3 64bit', u'os_type': u'Linux'}, 'size': 787726336}
image_service：
 <cinder.image.glance.GlanceImageService object at 0x7f6d37f6a710>
kwargs：
 {'status': u'creating', 'volume_size': 10, 'volume_name': 'volume-eddfec51-9bc5-4649-8050-96ef7b909b30'}




重要流程调用（一）：
if not volume_is_encrypted:
model_update, cloned = self.driver.clone_image(context,
					       volume,
					       image_location,
					       image_meta,
					       image_service)
此处调用cinder的南向插件，创建磁盘。创建完成后：
model_update：
cloned：False

重要流程调用（二）：
if not cloned and 'cinder' in CONF.allowed_direct_url_schemes:
model_update, cloned = self._clone_image_volume(context,
						volume,
						image_location,
						image_meta)

CONF.allowed_direct_url_schemes  表示“A list of url scheme that can be downloaded directly via the direct_url.  Currently supported schemes: [file]”
此处表示如果非cloned并且cinder可以直接从glance后端下载镜像，则直接调用_clone_image_volue方法。


重要流程调用（三）：
if self.image_volume_cache and not cloned:
internal_context = cinder_context.get_internal_tenant_context()
if not internal_context:
    LOG.info(_LI('Unable to get Cinder internal context, will '
		 'not use image-volume cache.'))
else:
    model_update, cloned = self._create_from_image_cache(
	context,
	internal_context,
	volume,
	image_id,
	image_meta
    )



重要流程调用（四）：
with image_utils.TemporaryImages.fetch(
    image_service, context, image_id) as tmp_image:

# Try to create the volume as the minimal size, then we can
# extend once the image has been downloaded.
data = image_utils.qemu_img_info(tmp_image)

virtual_size = image_utils.check_virtual_size(
    data.virtual_size, volume.size, image_id)

if should_create_cache_entry:
    if virtual_size and virtual_size != original_size:
	volume.size = virtual_size
	volume.save()
	model_update = self._create_from_image_download(
				context,
				volume,
				image_location,
				image_id,
				image_service
			    )

之后调用如下方法，主要完成两部分内容：
1、调用插件借口创建磁盘self.driver.create_volume(volume)，返回model_update内容为：
						model_update = self.driver.create_volume(volume) or {}

 {'provider_location': u'addr=10.100.0.6:7443,uri=/service/sites/3800067A/volumes/528,urn=urn:sites:3800067A:volumes:528,datastoreUrn=urn:sites:3800067A:datastores:3,isThin=True,storageType=LOCALPOME,type=normal'}

2、更新数据库中磁盘创建状态为 downloading
						model_update['status'] = 'downloading'
3、调用driver的copy_image_to_volume接口，镜像拷贝卷。			
						self._copy_image_to_volume(context, volume, image_id, image_location,image_service)



重要流程调用（五）：
self._handle_bootable_volume_glance_meta(context, volume,
						 image_id=image_id,
						 image_meta=image_meta)


if not cloned and 'cinder' in CONF.allowed_direct_url_schemes:
    model_update, cloned = self._clone_image_volume(context,
						    volume,
						    image_location,
						    image_meta)



	
```
